<template>
  <div class="container">
    <Controller />
    <GamePlay />
  </div>
</template>

<script>
import Controller from "./Controller.vue";
import GamePlay from "./GamePlay.vue";

export default {
  components: {
    Controller,
    GamePlay,
  },
  setup() {},
};
</script>

<style scoped>
.container {
  background-color: #efefef;
  max-width: 1000px;
  margin: 50px auto;
  border-radius: 8px;
  align-items: center;
  justify-content: center;
  text-align: center;
}
</style>
