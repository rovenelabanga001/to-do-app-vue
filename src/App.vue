<script setup>
import { ref, computed } from "vue";

const today = ref(
  new Date().toLocaleString("en-UK", {
    weekday: "long",
    day: "numeric",
    month: "long",
    year: "numeric",
  })
);

const tasks = ref([
  { text: "Learn Vue 3", done: false },
  { text: "Build a To Do app", done: true },
]);

const newTask = ref("");

const addTask = () => {
  const trimmed = newTask.value.trim();
  if (trimmed) {
    tasks.value.push({ text: trimmed, done: false });
    newTask.value = "";
  }
};

const removeTask = (index) => {
  tasks.value.splice(index, 1);
};

const remainingTasks = computed(() => {
  return tasks.value.filter((task) => !task.done).length;
});

const toggleTask = (index) => {
  tasks.value[index].done = !tasks.value[index].done;
};
</script>

<template>
  <header>
    <h3 class="date">
      {{ today }}
    </h3>
  </header>
  <section class="app-content">
    <div class="app-name">
      <h5>To Do List 📝</h5>
    </div>
    <div class="task-input">
      <div>
        <input
          type="text"
          id="add-task"
          placeholder="Enter task"
          v-model="newTask"
          @keyup.enter="addTask"
        />
        <button @click="addTask">Add Task</button>
      </div>
      <p v-if="tasks.length">You have {{ remainingTasks }} task(s) remaining</p>
      <p v-else>Add a task</p>
    </div>
    <div class="tasks-container">
      <h5 id="tasks-heading">Tasks</h5>
      <ul>
        <li v-for="(task, index) in tasks" :key="index">
          <span :class="{ done: task.done }">{{ task.text }}</span>
          <span
            ><input
              type="checkbox"
              name=""
              id=""
              v-model="task.done"
              @click="toggleTask(index)" /></span
          ><button @click="removeTask(index)">x</button>
        </li>
      </ul>
      <p v-if="tasks.length === 0">No tasks yet, Add one above</p>
    </div>
  </section>
</template>

<style scoped></style>
