<template>
  <div class="board">
    <BoardItem
      :gameStatus="gameStatus"
      v-for="field in fields"
      :key="field.id"
      :field="field"
      @selectField="selectField($event)"
    />
  </div>
</template>

<script>
import BoardItem from "../components/BoardItem.vue";

export default {
  name: "Board",
  components: {
    BoardItem,
  },
  props: {
    fields: Array,
    gameStatus: String,
  },
  methods: {
    selectField(id) {
      this.$emit("selectField", id);
    },
  },
};
</script>

<style lang="scss" scoped>
.board {
  width: 300px;
  height: 300px;
  display: grid;
  grid-template-columns: 1fr 1fr;
  border-radius: 100%;
  overflow: hidden;
}
</style>
