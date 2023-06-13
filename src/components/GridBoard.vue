<template>
  <div class="grid-board">
    <h3>Flipper Game</h3>
    <p>Turn all tiles to white!</p>
    <div class="grid-container">
      <div v-for="row in numRows" :key="row" class="grid-column">
        <div
          v-for="(box, index) in getBoxesInRow(row)"
          :key="index"
          class="grid-box"
          :class="{
            green: box.color === 'green',
            white: box.color === 'white',
            circle: selectedTile === 'circle',
            rounded: selectedTile === 'rounded',
          }"
          @click="handleBoxClick(box)"
        >
          {{ box.row }}, {{ box.column }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { computed, ref, watch } from "vue";

export default {
  props: {
    selectedLevel: {
      type: String,
      required: true,
    },
    selectedTile: {
      type: String,
      required: true,
    },
  },

  setup(props) {
    const numColumns = ref(4);
    const numRows = ref(4);
    const grid = ref([]);

    const getBoxesInRow = (row) => {
      return grid.value
        .filter((box) => box.row === row)
        .sort((a, b) => a.column - b.column);
    };

    const initializeGrid = () => {
      const columns = numColumns.value;
      const rows = numRows.value;

      grid.value = [];

      for (let row = 0; row <= rows; row++) {
        for (let column = 0; column < columns; column++) {
          const color = Math.random() < 0.5 ? "green" : "white";
          grid.value.push({ id: `${row}-${column}`, row, column, color });
        }
      }
    };

    const handleBoxClick = (box) => {
      const { row, column, color } = box;
      const adjacentBoxes = getAdjacentBoxes(row, column);

      adjacentBoxes.forEach((adjacentBox) => {
        if (adjacentBox.color === "white") {
          adjacentBox.color = "green";
        } else if (adjacentBox.color === "green") {
          adjacentBox.color = "white";
        }
      });

      if (color === "white") {
        box.color = "green";
      } else if (color === "green") {
        box.color = "white";
      }
    };

    const getAdjacentBoxes = (row, column) => {
      const adjacentBoxes = [];

      if (column > 0) {
        adjacentBoxes.push(
          grid.value.find((box) => box.row === row && box.column === column - 1)
        );
      }

      if (column < numColumns.value - 1) {
        adjacentBoxes.push(
          grid.value.find((box) => box.row === row && box.column === column + 1)
        );
      }

      if (row > 0) {
        adjacentBoxes.push(
          grid.value.find((box) => box.row === row - 1 && box.column === column)
        );
      }

      if (row < numRows.value - 1) {
        adjacentBoxes.push(
          grid.value.find((box) => box.row === row + 1 && box.column === column)
        );
      }

      return adjacentBoxes;
    };

    watch(
      () => props.selectedLevel,
      (newLevel) => {
        switch (newLevel) {
          case "easy":
            numColumns.value = 4;
            numRows.value = 4;
            break;
          case "hard":
            numColumns.value = 5;
            numRows.value = 5;
            break;
          case "nightmare":
            numColumns.value = 6;
            numRows.value = 6;
            break;
          default:
            numColumns.value = 0;
            numRows.value = 0;
            break;
        }

        initializeGrid();
      }
    );

    initializeGrid();

    return {
      numColumns,
      numRows,
      getBoxesInRow,
      handleBoxClick,
    };
  },
};
</script>

<style>
.grid-container {
  display: flex;
  flex-direction: column;
  margin: 10px auto;
  align-items: center;
  justify-content: center;
  text-align: center;
}
.grid-column {
  display: flex;
  flex-direction: row;
  gap: 10px;
  margin-right: 10px;
}

.grid-box {
  height: 70px;
  width: 70px;
  margin-bottom: 10px;
  background-color: var(--box-color);
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
}

.green {
  --box-color: green;
}

.white {
  --box-color: white;
}

.circle {
  border-radius: 50%;
}

.rounded {
  border-radius: 5px;
}
</style>
