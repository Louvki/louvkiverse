<template>
  <div class="terminal">
    <div class="terminal-header">
      <div class="buttons">
        <span class="red"></span>
        <span class="yellow"></span>
        <span class="green"></span>
      </div>
    </div>
    <div class="terminal-body">
      <div class="matrix-wrapper">
        <div
          v-for="(line, index) in matrixLines"
          :key="index"
          class="matrix-line"
        >
          <span
            v-for="(char, charIndex) in line"
            :key="charIndex"
            :style="{ color: getRandomColor(charIndex) }"
          >
            {{ char }}
          </span>
        </div>
      </div>
      <p v-if="showQuote" class="quote">
        "There's a difference between knowing the path and walking the path"
      </p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      matrixLines: Array(20).fill([]), // 20 rows
      showQuote: false,
      characters: "ABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890"
    };
  },
  mounted() {
    this.startMatrixEffect();
  },
  methods: {
    getRandomCharacter() {
      return this.characters.charAt(
        Math.floor(Math.random() * this.characters.length)
      );
    },
    getRandomColor(index) {
      // Simulate a glow effect: more vibrant color at the end of the cascade
      return index > 5 ? "#0f0" : `rgba(0, 255, 0, ${(index + 1) / 6})`;
    },
    startMatrixEffect() {
      let interval = setInterval(() => {
        this.matrixLines = this.matrixLines.map((line) => [
          this.getRandomCharacter(),
          ...line.slice(0, 50) // Keep each line max 50 chars long
        ]);
      }, 100);

      setTimeout(() => {
        clearInterval(interval);
        this.showQuote = true;
      }, 5000); // Show the quote after 5 seconds
    }
  }
};
</script>

<style scoped>
.terminal {
  background-color: #000;
  color: #0f0;
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  font-family: 'Courier New', monospace;
  overflow: hidden;
}

.terminal-header {
  display: flex;
  justify-content: flex-start;
  padding: 10px;
  background-color: #333;
}

.buttons {
  display: flex;
  gap: 5px;
}

.buttons span {
  width: 12px;
  height: 12px;
  border-radius: 50%;
}

.red {
  background-color: #ff5f56;
}

.yellow {
  background-color: #ffbd2e;
}

.green {
  background-color: #27c93f;
}

.terminal-body {
  padding: 20px;
  height: calc(100% - 40px);
  overflow: hidden;
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.matrix-wrapper {
  display: flex;
  flex-direction: column;
  gap: 2px;
  overflow: hidden;
}

.matrix-line {
  font-size: 18px;
  white-space: pre;
  overflow: hidden;
  display: flex;
  line-height: 1.2em;
}

.quote {
  position: absolute;
  bottom: 20px;
  left: 20px;
  font-size: 24px;
  color: #0f0;
  text-align: center;
}
</style>
