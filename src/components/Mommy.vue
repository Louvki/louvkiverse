<template>
  <div>
    <TerminalTyping
      v-if="showTerminal"
      :username="username"
      :sentence="sentence"
      @typingComplete="handleTypingComplete"
    />
    <Matrix2 v-else />
  </div>
</template>

<script>
import TerminalTyping from './TerminalTyping.vue';
import Matrix2 from './Matrix2.vue';

export default {
  components: {
    TerminalTyping,
    Matrix2,
  },
  data() {
    return {
      username: 'louvkiverse@MacBook-Pro ~ % ',
      sentence: "There's a difference between knowing the path and walking the path...",
      showTerminal: true,
    };
  },
  methods: {
    handleTypingComplete() {
      setTimeout(() => {
        this.showTerminal = false;
      }, 3000);
    }
  }
};
</script>

<style scoped>
/* Styles as needed */
</style>
