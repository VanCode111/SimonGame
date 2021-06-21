<template>
  <div
    @click="select(field.id)"
    :class="`board-item ${field.value == 1 ? 'active' : ''}`"
    :style="`background-color: ${field.color}`"
  ></div>
</template>

<script>
export default {
  name: "Board",
  props: {
    field: Object,
    gameStatus: String,
  },
  methods: {
    select(id) {
      if (this.gameStatus == "process") this.$emit("selectField", id);
    },
  },
};
</script>

<style lang="scss" scoped>
.board-item {
  opacity: 0.5;
  cursor: pointer;
  &.active {
    opacity: 1;
  }
  &:disabled {
    cursor: unset;
  }
}
</style>
