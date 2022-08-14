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

const removeTask = (id: number) => {
  const idx = todos.findIndex((todo) => todo.id === id);
  todos.splice(idx, 1);
};

const completeTask = (id: number) => {
  const idx = todos.findIndex((todo) => todo.id === id);
  todos[idx].status = "complete";
};
</script>

<template>
  <div>
    <h1>Todos</h1>
    <form @submit.prevent>
      <input type="text" v-model="newTask" />
      <button @click="addTask">+</button>
    </form>
    <section v-if="todos.length">
      <h2 v-if="pendingTodos.length">Pending</h2>
      <ul>
        <li v-for="(todo, i) in pendingTodos" :key="i">
          <div class="task-wrapper">
            <h3>{{ todo.task }}</h3>

            <div id="buttons" class="btn-wrapper">
              <button class="btn-action" @click="removeTask(todo.id)">
                <span class="btn-text"> ❌ </span>
              </button>
              <button class="btn-action" @click="completeTask(todo.id)">
                <span class="btn-text"> ✅ </span>
              </button>
            </div>
          </div>
        </li>
      </ul>
      <hr v-if="completedTodos.length && pendingTodos.length" />
      <h2 v-if="completedTodos.length">Completed</h2>
      <ul id="complete-todos">
        <li v-for="(todo, i) in completedTodos" :key="i">
          <div class="task-wrapper">
            <h3>{{ todo.task }}</h3>

            <div id="buttons" class="btn-wrapper">
              <button class="btn-action-complete" @click="removeTask(todo.id)">
                <span class="btn-text"> ❌ </span>
              </button>
            </div>
          </div>
        </li>
      </ul>
    </section>
    <h2 v-else>No Tasks</h2>
  </div>
</template>

<style>
@import url("https://cdn.jsdelivr.net/npm/water.css@2/out/dark.min.css");
h1 {
  width: 100%;
  text-align: center;
}
h2 {
  text-align: center;
}
h3 {
  margin: auto 0;
}
ul {
  padding: 0 1rem;
}
li {
  list-style: none;
}

form {
  display: flex;
}

form input {
  width: 100%;
  font-size: 3rem;
}

form buttono {
  font-size: 2rem;
  color: gray;
}

.btn-wrapper {
  display: flex;
  width: 110px;
}
.btn-text {
  margin: auto;
}
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
