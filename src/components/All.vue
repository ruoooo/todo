<template>
  <div>
    <li
      class="todo-item"
      v-for="(item, index) in todos"
      :key="index"
      @mouseenter="showDeleteButton=index"
      @mouseleave="showDeleteButton=-1"
    >
      <div class="complete-button-wrapper" @click="makeCompleted(index)">O</div>
      <p :class="{completed:item.completed}">{{item.title}}</p>
      <button class="delete" @click="deleteItem(index)" v-if="index===showDeleteButton">delete</button>
    </li>
  </div>
</template>
<script>
export default {
  props: ["todos"],
  data() {
    return {
      showDeleteButton: -1,
    };
  },
  methods: {
    makeCompleted: function (index) {
      this.$emit("makeCompleted", index);
    },
    deleteItem: function (index) {
      this.$emit("deleteItem", index);
    },
  },
};
</script>