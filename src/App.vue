<template>
  <div class="container">
    <MedalTinder />
  </div>
</template>

<script>
import MedalTinder from '../src/components/MedalTinder'

export default {
  name: 'App',
  components: {
    MedalTinder,
  }
}
</script>

<style>
  .container {
    max-width: 500px;
    padding-top: 50px;
  }
</style>