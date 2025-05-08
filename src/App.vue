<script setup>
import { ref, computed, onMounted } from 'vue';

const tasks = ref([
  { name: "Latihan break shot", done: false },
  { name: "Menonton pertandingan bilyard profesional", done: true },
  { name: "Berlatih kontrol bola", done: false },
  { name: "Mempelajari teknik spin", done: true }
]);

const newTask = ref("");
const inputField = ref(null);

const isEditing = ref(false);
const editIndex = ref(null);
const showCompleted = ref(true);

const addTask = () => {
  const trimmed = newTask.value.trim();
  if (trimmed) {
    tasks.value.push({ name: trimmed, done: false });
    newTask.value = "";
    inputField.value?.focus();
  }
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
  cancelEdit();
};

const editTask = (index) => {
  newTask.value = tasks.value[index].name;
  isEditing.value = true;
  editIndex.value = index;
  inputField.value?.focus();
};

const updateTask = () => {
  const trimmed = newTask.value.trim();
  if (editIndex.value !== null && trimmed) {
    tasks.value[editIndex.value].name = trimmed;
    cancelEdit();
  }
};

const cancelEdit = () => {
  newTask.value = "";
  isEditing.value = false;
  editIndex.value = null;
};

const filteredTasks = computed(() =>
  showCompleted.value
    ? tasks.value
    : tasks.value.filter(task => !task.done)
);

onMounted(() => {
  inputField.value?.focus();
});
</script>

<template>
  <div id="app">
    <div class="container">
      <div class="card">
        <h2>🎱 Agenda Latihan Bilyard</h2>

        <label class="toggle-show">
          <input type="checkbox" v-model="showCompleted" />
          Tampilkan yang sudah selesai
        </label>

        <ul class="task-list">
          <li v-for="(task, index) in filteredTasks" :key="index">
            <div class="task-content">
              <input type="checkbox" v-model="task.done" />
              <span :class="{ done: task.done }">{{ index + 1 }}. {{ task.name }}</span>
            </div>
            <div class="action-buttons">
              <button class="edit-btn" @click="editTask(index)">✏</button>
              <button class="delete-btn" @click="deleteTask(index)">🗑</button>
            </div>
          </li>
        </ul>

        <div class="input-group">
          <input
            ref="inputField"
            v-model="newTask"
            type="text"
            placeholder="Tambahkan agenda bilyard"
          />
          <button v-if="isEditing" @click="updateTask">Update</button>
          <button v-else @click="addTask">Tambah</button>
          <button v-if="isEditing" class="cancel-btn" @click="cancelEdit">Batal</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
</style>
