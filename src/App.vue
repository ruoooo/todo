<template>
  <div id="app">
    <div class="toggle-button-wrapper">
      <button class="toggle" @click="toggleAll()" v-if="showList">v</button>
    </div>
    <input type="text" @keyup.enter="addTodo($event)" />
    <div class="list-container" v-if="showList">
      <All
        :todos="todos"
        class="all"
        v-show="nowShowing==='all'"
        @deleteItem="deleteItem"
        @makeCompleted="makeCompleted"
      ></All>
      <div class="button-wrapper">
        <p class="item-counting">{{activeItem}} item left</p>
        <div to="/all" class="filter-button" @click="nowShowing='all'">all</div>
        <div to="/active" class="filter-button" @click="nowShowing='active'">active</div>
        <div to="/completed" class="filter-button" @click="nowShowing='completed'">completed</div>
        <div class="filter-button clear" @click="clear()" v-if="completedItem!==0">clear completed</div>
      </div>
    </div>
  </div>
</template>

<script>
import All from "./components/All.vue";

export default {
  name: "App",
  components: {
    All,
  },
  data() {
    return {
      todos: [
        {
          title: "walking the dog",
          completed: false,
        },
      ],
      showList: true,
      nowShowing: "all",
      activeItem: 1,
      completedItem: 0,
    };
  },

  mounted() {
    if (this.todos.length === 0) {
      this.showList = false;
    }
  },
  methods: {
    addTodo: function (e) {
      let newTodo = { title: e.target.value, completed: false };
      this.todos.push(newTodo);
      e.target.value = "";

      if (!this.showList) {
        this.showList = true;
      }
      this.activeItem++;
    },
    deleteItem: function (index) {
      console.log(index);
      let item = this.todos[index];
      this.todos = this.todos
        .slice(0, index)
        .concat(this.todos.slice(index + 1));
      if (this.todos.length === 0) {
        this.showList = false;
      }
      if (item.completed === false) {
        this.activeItem--;
      } else {
        this.completedItem--;
      }
    },
    makeCompleted: function (index) {
      let item = this.todos[index];
      if (item.completed === false) {
        this.activeItem--;
        this.completedItem++;
      }

      this.todos[index].completed = true;
    },
    clear: function () {
      this.todos = this.todos.filter((i) => !i.completed);
      if (this.todos.length === 0) {
        this.showList = false;
      }
      this.completedItem = 0;
    },
    toggleAll: function () {
      if (this.todos.length === this.completedItem) {
        this.todos.forEach((i) => {
          i.completed = false;
        });
        this.completedItem = 0;
        this.activeItem = this.todos.length;
      } else {
        this.todos.forEach((i) => {
          i.completed = true;
        });
        this.completedItem = this.todos.length;
        this.activeItem = 0;
      }
    },
  },
  watch: {
    activeItem: function () {
      if (this.completedItem === 0) {
        this.showDeleteButton = -1;
      }
    },
  },
};
</script>

<style>
* {
  padding: 0px;
  margin: 0px;
}
body {
  background-color: rgb(234, 234, 234);
  /* font-family: "Work Sans", sans-serif; */
  font-size: 20px;
}
header h1 {
  font-size: 64px;
  margin: 2rem 0rem;
  text-align: center;
}
#app {
  border: 1px solid lightgray;
  width: 600px;
  margin: auto;
  background-color: #fff;
}
input {
  width: 90%;
  box-sizing: border-box;
  border: none;
  /* border-bottom: 1px lightgray solid; */
  outline: none;
  height: 3.5rem;
  font-size: 1.5rem;
  display: inline-block;
  line-height: 3.5rem;
}
.toggle-button-wrapper {
  width: 6%;
  display: inline-block;
}
.toggle {
  width: 100%;
  background: none;
  border: none;
}

.todo-item {
  font-size: 1.5rem;
  list-style: none;
  border-top: lightgray solid 1px;
  position: relative;
}

.complete-button-wrapper {
  width: 9%;
  display: inline-block;
  height: 100%;
}

.todo-item p {
  display: inline-block;
  /* width: 80%; */
  padding: 1rem 1rem;
}

.completed {
  text-decoration: line-through;
}

.delete {
  position: absolute;
  right: 0px;
}

.button-wrapper {
  display: flex;
  justify-content: center;
  height: 3rem;
  position: relative;
  font-size: 1rem;
  align-items: center;
  border-top: lightgray solid 1px;
}
.filter-button {
  display: inline-block;
  padding: 0.2rem;
  margin: 0px 10px;
  box-sizing: border-box;
}
.filter-button:hover {
  border: 1px solid lightgray;
  border-radius: 10px;
}
.clear {
  position: absolute;
  right: 0px;
}
.item-counting {
  position: absolute;
  left: 0px;
  padding: 0.2rem;
}
</style>
