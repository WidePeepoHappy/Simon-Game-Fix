<template>
  <h1>Simon Game</h1>
  <div class="container">
    <Circle :mode="mode"></Circle>
    <Settings @changeDiff="changeDiff"></Settings>
  </div>
</template>

<script>
import Circle from "./components/Circle.vue";
import Settings from "./components/Settings.vue";
export default {
  components: {
    Circle,
    Settings,
  },
  data() {
    return {
      mode: "easy",
    };
  },
  methods: {
    changeDiff(diff) {
      this.mode = diff;
    },
  },
};
</script>

<style lang="scss">
h1 {
  text-align: center;
}

.container {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
