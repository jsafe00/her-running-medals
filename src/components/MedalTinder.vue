<template>
  <div id="app">
    <Tinder ref="tinder" key-name="id" :queue.sync="queue" :offset-y="10" @submit="onSubmit">
      <template slot-scope="scope">
        <div
          class="pic"
          :style="{
            'background-image': 'url(' + require(`../assets/medals/${scope.data.id}.jpg`) + ')'
        }"
        />
      </template>
      <img class="like-pointer" slot="like" src="../assets/like-txt.png">
      <img class="nope-pointer" slot="nope" src="../assets/nope-txt.png">
      <img class="rewind-pointer" slot="rewind" src="../assets/rewind-txt.png">
    </Tinder>
    <div class="btns">
      <img src="../assets/rewind.png" @click="decide('rewind')">
      <img src="../assets/nope.png" @click="decide('nope')">
      <img src="../assets/like.png" @click="decide('like')">
      <img src="../assets/help.png" @click="decide('help')">
    </div>
    <div class="footer">
      <!-- to do  -->
    </div>
  </div>
</template>

<script>
import Tinder from "vue-tinder";
import source from "@/medals";

export default {
  name: "App",
  components: { Tinder },
  data: () => ({
    queue: [],
    offset: 0,
    history: [],
  }),
  created() {
    this.mock();
  },
  methods: {
    mock(count = 5, append = true) {
      const list = [];
      for (let i = 0; i < count; i++) {
        list.push({ id: source[this.offset] });
        this.offset++;
      }
      if (append) {
        this.queue = this.queue.concat(list);
      } else {
        this.queue.unshift(...list);
      }
    },
    onSubmit({ item }) {
      if (this.queue.length < 3) {
        this.mock();
      }
      this.history.push(item);
    },
    async decide(choice) {
      if (choice === "rewind") {
        if (this.history.length) {
          this.$refs.tinder.rewind([this.history.pop()]);
        }
      } else if (choice === "help") {
        window.open("https://github.com/jsafe00/her-running-medals");
      } else {
        this.$refs.tinder.decide(choice);
      }
    }
  }
};
</script>

<style>
html {
  max-width: 100%;
  overflow-x: hidden; 
}

body {
  margin: 0;
  background-color: #20262e;
  padding:50px;
  display:flex;
  max-width: 100%;
}

a { 
    color: white;
}

#app .vue-tinder {
  position: absolute;
  z-index: 1;
  left: 0;
  right: 0;
  top: 23px;
  margin: auto;
  width: calc(100% - 20px);
  height: calc(100% - 23px - 65px - 47px - 16px);
  min-width: 355px;
  max-width: 355px;
}

.nope-pointer,
.like-pointer {
  position: absolute;
  z-index: 1;
  top: 20px;
  width: 64px;
  height: 64px;
}

.nope-pointer {
  right: 10px;
}

.like-pointer {
  left: 10px;
}

.rewind-pointer {
  position: absolute;
  z-index: 1;
  top: 20px;
  right: 100px;
  width: 112px;
  height: 78px;
}

.pic {
  width: 100%;
  height: 100%;
  background-size: cover;
  background-position: center;
}

.btns {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 30px;
  margin: auto;
  height: 65px;
  display: flex;
  align-items: center;
  justify-content: center;
  min-width: 300px;
  max-width: 355px;
}

.footer{
 position: fixed;
  left: 0;
  right: 0;
  color:white;
  bottom: 0;
  margin: auto;
  height: 35px;
  display: flex;
  align-items: center;
  justify-content: center;
  min-width: 300px;
  max-width: 355px;
  font-family: arial;
  font-size: 11px;
}

.btns img {
  margin-right: 12px;
  box-shadow: 0 4px 9px rgba(0, 0, 0, 0.15);
  border-radius: 50%;
  cursor: pointer;
  -webkit-tap-highlight-color: transparent;
}

.btns img:nth-child(2n + 1) {
  width: 53px;
}

.btns img:nth-child(2n) {
  width: 65px;
}

.btns img:nth-last-child(1) {
  margin-right: 0;
}

@media screen and (max-width: 1000px) {
  html, body { 
    max-width: 100% !important; 
    overflow-x: hidden !important; 
    } 
}

</style>


