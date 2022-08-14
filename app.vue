<script setup lang="ts">
interface ITodo {
  id: number;
  task: string;
  status: "pending" | "complete";
}

const todos = reactive<ITodo[]>([]);

const pendingTodos = computed<ITodo[]>(() =>
  todos.filter((todo) => todo.status === "pending")
);

const completedTodos = computed<ITodo[]>(() =>
  todos.filter((todo) => todo.status === "complete")
);

const newTask = ref("");

const addTask = () => {
  if (!newTask.value.length) return;
  todos.push({
    id: todos.length + 1,
    task: newTask.value,
    status: "pending",
  });
  newTask.value = "";
};

const removeTask = (i: number) => todos.splice(i, 1);

const completeTask = (i: number) => (todos[i].status = "complete");
</script>

<template>
  <div>
    <h1 style="width: 100%; text-align: center">Todos</h1>
    <form @submit.prevent style="display: flex">
      <input
        style="width: 100%; font-size: 3rem"
        type="text"
        v-model="newTask"
      />
      <button style="font-size: 2rem; color: gray" @click="addTask">+</button>
    </form>
    <section v-if="todos.length">
      <h2 v-if="pendingTodos.length" style="text-align: center">Pending</h2>
      <ul style="padding: 0 1rem">
        <li v-for="(todo, i) in pendingTodos" :key="i" style="list-style: none">
          <div class="task-wrapper">
            <h3 style="margin: auto 0">{{ todo.task }}</h3>

            <div id="buttons" style="display: flex; width: 110px">
              <button class="btn-action" @click="removeTask(i)">
                <span style="margin: auto"> ❌ </span>
              </button>
              <button class="btn-action" @click="completeTask(i)">
                <span style="margin: auto"> ✅ </span>
              </button>
            </div>
          </div>
        </li>
      </ul>
      <hr v-if="completedTodos.length && pendingTodos.length" />
      <h2 v-if="completedTodos.length" style="text-align: center">Completed</h2>
      <ul id="complete-todos" style="padding: 0 1rem">
        <li
          v-for="(todo, i) in completedTodos"
          :key="i"
          style="list-style: none"
        >
          <div class="task-wrapper">
            <h3 style="margin: auto 0">{{ todo.task }}</h3>

            <div id="buttons" style="display: flex; width: 110px">
              <button class="btn-action-complete" @click="removeTask(i)">
                <span style="margin: auto"> ❌ </span>
              </button>
            </div>
          </div>
        </li>
      </ul>
    </section>
    <h2 style="text-align: center" v-else>No Tasks</h2>
  </div>
</template>

<style>
@import url("https://cdn.jsdelivr.net/npm/water.css@2/out/dark.min.css");

.btn-action {
  padding: 0;
  width: 50px;
  height: 50px;
  margin: auto;
  background-color: blueviolet;
  display: flex;
}

.btn-action-complete {
  padding: 0;
  width: 50px;
  height: 50px;
  margin: auto 0 auto auto;
  background-color: blueviolet;
  display: flex;
}

.task-wrapper {
  display: flex;
  justify-content: space-between;
  margin: 1rem auto;
}
</style>
