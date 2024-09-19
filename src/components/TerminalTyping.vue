<template>
  <div class="terminal-container">
    <div class="terminal">
      <p>{{ terminalText }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    username: {
      type: String,
      default: 'user@MacBook-Pro ~ % '
    },
    sentence: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      terminalText: this.username,
      typingIndex: 0
    };
  },
  mounted() {
    setTimeout(() => {
      this.startTypingEffect();
    }, 1500)
  },
  methods: {
    startTypingEffect() {
      const typingSpeed = 56; // Time between each letter (in ms)
      const sentence = this.sentence;

      const typeSentence = () => {
        if (this.typingIndex < sentence.length) {
          this.terminalText += sentence.charAt(this.typingIndex);
          this.typingIndex++;
          setTimeout(typeSentence, typingSpeed);
        } else {
          // Emit the event when typing finishes
          this.$emit('typingComplete');
        }
      };

      typeSentence();
    }
  }
};
</script>

<style scoped>
.terminal-container {
  width: 100vw;
    height: 100vh;
    overflow: hidden;
    margin: 0;
    padding: 0;
    position: fixed;
    top: 0;
    left: 0;
    background-color: black;
}

.terminal {
  color: white;
  font-family: monospace;
  white-space: pre-wrap;
  font-size: 1.5rem;
  text-align: left;
  width: 100%;
  min-width: 100%;
  padding: 20px;
}
</style>
