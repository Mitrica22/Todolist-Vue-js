<script>
export default {
  data() {
    return {
      todos: [],
      newTodo: { done: false },
    };
  },
  methods: {
    addTodo() {
      if (this.newTodo.text) {
        this.todos.push(this.newTodo);
        this.newTodo = {
          done: false,
        };
        localStorage.setItem("todos", JSON.stringify(this.todos));
      } else {
        alert("To-do text is required");
      }
    },
    removeTodo(todoText) {
      this.todos = this.todos.filter((todo) => todo.text !== todoText);
    },
    updateTodoInLocalstorage() {
      localStorage.setItem("todos", JSON.stringify(this.todos));
    },
  },
  created() {
    this.todos = localStorage.getItem("todos")
      ? JSON.parse(localStorage.getItem("todos"))
      : this.todos;
  },
};
</script>

<!-- Uncomment below to use Composition Api -->
<!-- <script setup>
import { onMounted, ref } from "vue";

const todos = ref([]);
const newTodo = ref({
  done: false,
});

function addTodo() {
  if (newTodo.value.text) {
    todos.value.push(newTodo.value);
    newTodo.value = {
      done: false,
    };
    localStorage.setItem("todos", JSON.stringify(todos.value));
  } else {
    alert("To-do text is required");
  }
}

function removeTodo(todoText) {
  todos.value = todos.value.filter((todo) => todo.text !== todoText);
}

function updateTodoInLocalstorage() {
  localStorage.setItem("todos", JSON.stringify(todos.value));
}

onMounted(() => {
  todos.value = localStorage.getItem("todos")
    ? JSON.parse(localStorage.getItem("todos"))
    : todos;
});
</script> -->

<template>
  <main>
    <div class="todoList">
      <h3>To-do List</h3>

      <div class="todos">
        <div
          class="todo"
          v-for="todo in todos"
          :class="{ done: todo.done }"
          @click="(todo.done = !todo.done), updateTodoInLocalstorage()"
        >
          <p>{{ todo.text }}</p>
          <button
            class="remove-todo"
            @click="removeTodo(todo.text), updateTodoInLocalstorage()"
          >
            X
          </button>
        </div>
      </div>

      <button
        v-if="todos.length"
        class="clear-todos"
        @click="(todos = []), updateTodoInLocalstorage()"
      >
        Clear All
      </button>

      <input type="text" placeholder="Add new to-do" v-model="newTodo.text" />
      <button class="add-todo" @click="addTodo(), updateTodoInLocalstorage()">
        Add
      </button>
    </div>
  </main>
</template>

<style scoped>
body {
  font-family: "Open Sans", sans-serif;
  margin: 0;
}

main {
  align-items: flex-start;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  padding-top: 40px;
}

main .todoList input {
  box-sizing: border-box;
  border: 1px solid lightgrey;
  border-radius: 0.25rem;
  height: 28px;
  margin-top: 32px;
  width: 80%;
}

main button {
  background-color: transparent;
  border-radius: 0.25rem;
  box-sizing: border-box;
  color: #fff;
  cursor: pointer;
  height: 28px;
}

main button:hover {
  opacity: 0.8;
}

main button.add-todo {
  background-color: #007bff;
  border: 1px solid #007bff;
  margin-left: 2px;
}

main button.remove-todo {
  background-color: #dc3545;
  border: 1px solid #dc3545;
  margin-left: 2px;
}

main button.clear-todos {
  background-color: #dc3545;
  border: 1px solid #dc3545;
  display: block;
  margin: auto;
}

main > div.todoList h3 {
  text-align: center;
  margin-top: 20px;
  width: 100%;
}

main > div.todoList {
  background-color: rgb(248, 248, 248);
  border: 1px solid lightgrey;
  border-radius: 0.25rem;
  flex-wrap: wrap;
  max-width: 500px;
  min-width: 300px;
  padding: 20px;
  text-align: center;
}

main .todos > *:not(:last-child) {
  border-bottom: 1px solid silver;
}

main .todos .todo {
  align-items: center;
  display: flex;
  justify-content: space-between;
}

main .todos .todo p {
  cursor: pointer;
  margin: 12px 0;
}

main .todos .todo.done p {
  text-decoration: line-through;
}

main .todos .todo button.remove {
  background: transparent url("img/remove.png") no-repeat center;
  background-size: contain;
  border: none;
  cursor: pointer;
  height: 12px;
  margin-left: 8px;
  width: 12px;
}

main > div.todoList button.clear-todos {
  margin-top: 24px;
}
</style>
