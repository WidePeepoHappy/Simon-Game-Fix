<template>
  <div>
    <h2>Сложность:</h2>
    <div id="difficulty">
      <label for="easy">Легкая</label>
      <input
        checked
        @change="$emit('changeDiff', 'easy')"
        type="radio"
        class="toggle"
        id="easy"
        name="difficulty"
      />
      <label for="medium">Средняя</label>
      <input
        @change="$emit('changeDiff', 'medium')"
        type="radio"
        class="toggle"
        id="medium"
        name="difficulty"
      />
      <label for="hard">Сложная</label>
      <input
        @change="$emit('changeDiff', 'hard')"
        type="radio"
        class="toggle"
        id="hard"
        name="difficulty"
      />
    </div>
  </div>
</template>

<script>
export default {
  props: ["turn"],
  emits: ["changeDiff"],
};
</script>

<style lang="scss">
#difficulty {
  font-size: 20px;
}

.toggle {
  margin-right: 10px;
}
</style>