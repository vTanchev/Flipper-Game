<template>
  <div>
    <h2>Game Play</h2>
    <div>
      <label for="playerName">Player Name:</label>
      <input type="text" id="playerName" v-model="playerName" />
    </div>
    <button @click="saveScore">Save Score</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      playerName: "",
      score: 100,
    };
  },
  methods: {
    saveScore() {
      if (this.playerName) {
        const playerData = {
          name: this.playerName,
          score: this.score,
        };
        localStorage.setItem("playerData", JSON.stringify(playerData));
        console.log("Score saved");
      } else {
        console.log("Please enter a name");
      }
    },
  },
};
</script>
