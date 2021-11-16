<template>
  <main>
    <h1>Tâches de la journée</h1>

    <form @submit.prevent="addTodos()">
      <label for="">Nouvelle tâche</label>
      <input type="text" v-model="toDoLists.todos" />

      <button type="submit" :disabled="!toDoLists.todos > 0">Ajouter</button>
    </form>

    <h2>Tâches à faire</h2>
    <h4 v-if="toDoLists.length == 0">
      Vous n'avez plus de tâches à faire... 😎
    </h4>
    <ul v-else>
      <li v-for="(toDoList, index) in toDoLists" :key="index">
        <span
          v-bind:class="{ 'task-done': toDoLists[index].isTaskDone }"
          @click="toggleDoneTask(index)"
          >{{ toDoList.todos }}</span
        >
        <button @click="deleteTask(index)">Supprimer</button>
      </li>
    </ul>
  </main>
</template>
<script>
export default {
  name: "Todo",

  data() {
    return {
      toDoLists: [],
    };
  },

  methods: {
    addTodos() {
      this.toDoLists.push({ todos: this.toDoLists.todos, isTaskDone: false });
      this.toDoLists.todos = "";
      this.saveTodos();
    },
    toggleDoneTask(index) {
      if (this.toDoLists[index].isTaskDone) {
        this.toDoLists[index].isTaskDone = false;
      } else {
        this.toDoLists[index].isTaskDone = true;
      }
      this.saveTodos();
    },
    deleteTask(index) {
      this.toDoLists.splice(index, 1);
      localStorage.removeItem("toDoLists", index);
      this.saveTodos();
    },
    saveTodos() {
      const storageData = JSON.stringify(this.toDoLists);
      localStorage.setItem("todos", storageData);
    },
  },
};
</script>

<style scoped>
.task-done {
  text-decoration: line-through;
}
</style>