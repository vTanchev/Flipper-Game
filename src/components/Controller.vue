<template>
  <div>
    <div class="controls">
      <button @click="restart">Restart</button>
      <div class="controller">
        <label for="difficulty">Difficulty:</label>
        <select
          id="difficulty"
          name="difficulty"
          v-model="selectedLevel"
          @change="onSelectedLevelChange"
        >
          <option value="easy" selected>Easy (4x4)</option>
          <option value="hard">Hard (5x5)</option>
          <option value="nightmare">Nightmare (6x6)</option>
        </select>
      </div>
      <div class="controller">
        <label for="box-tile-form">Tile form:</label>
        <select
          id="box-tile-form"
          name="box2-tile-form"
          v-model="selectedTile"
          @change="onSelectedTileChange"
        >
          <option value="square" selected>Square</option>
          <option value="circle">Circle</option>
          <option value="rounded">Slightly rounded</option>
        </select>
      </div>
    </div>
    <GridBoard
      :selectedLevel="selectedLevel"
      :selectedTile="selectedTile"
      :key="gameKey"
    />
  </div>
</template>

<script>
import { ref } from "vue";
import GridBoard from "./GridBoard.vue";

export default {
  components: {
    GridBoard,
  },
  setup() {
    const selectedLevel = ref("easy");
    const selectedTile = ref("square");
    const gameKey = ref(0);

    const onSelectedLevelChange = () => {
      console.log(selectedLevel.value);
    };

    const onSelectedTileChange = () => {
      console.log(selectedTile.value);
    };

    const restart = () => {
      gameKey.value++;
    };

    return {
      selectedLevel,
      onSelectedLevelChange,
      selectedTile,
      onSelectedTileChange,
      restart,
      gameKey,
    };
  },
};
</script>

<style scoped>
.controls {
  display: flex;
  flex-direction: column;
  gap: 10px;
  padding: 20px;
  max-width: 460px;
  margin: auto;
}
.controller {
  display: flex;
  flex-direction: column;
  width: 100%;
}

.controller label {
  text-align: left;
}

.controller select {
  outline: none;
  font-size: 16px;
  padding: 4px;
}

button {
  border: 1px solid #999;
  border-radius: 8px;
  padding: 0.6em 1.2em;
  font-size: 1em;
  font-weight: 500;
  background-color: #fff;
  cursor: pointer;
}

button:hover {
  border-color: #646cff;
}
</style>
