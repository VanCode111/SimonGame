<template>
  <div class="app">
    <Game />
  </div>
</template>

<script>
import Game from "./views/Game.vue";

export default {
  components: {
    Game,
  },
};
</script>

<style lang="scss">
body {
  font-family: sans-serif;
}
</style>
