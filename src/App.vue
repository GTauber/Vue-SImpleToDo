<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <h1>Vue TP01 App -> ToDoIst</h1>
  <button type="button" data-bs-toggle="modal" data-bs-target=".newTodoModal" class="btn btn-primary me-3">
    Add new ToDo
  </button>
  <div class="modal newTodoModal">

    <div class="modal-dialog modal-dialog-centered">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="exampleModalLabel">Add one new Task!</h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <form @submit.prevent="addNewTodo" id="form1">
            <h2> Name the task </h2>
            <input v-model="newToDo" type="text" name="newTodo" class="form-control" />
          </form>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
            Close
          </button>
          <button type="submit" form="form1" class="btn btn-primary" data-bs-dismiss="modal">
            Add ToDo
          </button>
        </div>
      </div>
    </div>

  </div>
  <button v-if="todos.length > 1" @click="toggleAllDone()">Select All</button>
  <div class="card mt-2" style="width: 100%;" v-if="todos.length">
    <ul class="list-group list-group-flush">
      <li v-for="(todo, index) in todos" :key="todo.id" class="list-group-item listItem" @click="finishTask(todo)"
      :class="{'li-done': todo.estaCompleta}">
        <input type="checkbox" v-model="todo.estaCompleta" />
        <span :class="{ done: todo.estaCompleta }">{{
            todo.descricao
        }}</span>
        <button @click="removeToDO(index)" class="btn" :class="{'btn-secondary': todo.estaCompleta,'btn-danger': !todo.estaCompleta}">Erase</button>
      </li>
    </ul>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    let allDone = false;
    const newToDo = ref("");
    const todos = ref([]);

    return {
      allDone,
      newToDo,
      todos,
      addNewTodo,
      finishTask,
      removeToDO,
      toggleAllDone,
    };

    function addNewTodo() {
      todos.value.push({
        id: Date.now(),
        descricao: newToDo.value,
        estaCompleta: false,
      });
      newToDo.value = "";
    }

    function finishTask(todo) {
      todo.estaCompleta = !todo.estaCompleta;
    }

    function removeToDO(index) {
      todos.value.splice(index, 1);
    }

    function toggleAllDone() {
      if (!allDone) {
        todos.value.forEach(
          (estaCompleta) => (estaCompleta.estaCompleta = true)
        );
        allDone = true;
        console.log(todos.value);
      } else {
        todos.value.forEach((todo) => (todo.estaCompleta = false));
        allDone = false
      }

    }

  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.done {
  text-decoration: line-through;
  color: red;
}

.li-done {
  background-color: lightgreen !important;
}

.container-form {
  display: flex;
  justify-content: center;
}

ul {
  display: flex;
  justify-content: center;
  align-items: center;
  list-style: none;
}

.listItem {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 70%;

}

.listItem:nth-child(2n) {
  background-color: lightgray;
}
</style>
