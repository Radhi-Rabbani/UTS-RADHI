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
html,
body,
#app {
  margin: 0;
  padding: 0;
  width: 100%;
  min-height: 100vh;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: #e3fafc;
  color: #212529;
  display: flex;
  justify-content: center;
  align-items: flex-start;
}

.container {
  flex: 1;
  display: flex;
  justify-content: center;
  padding: 40px 20px;
  box-sizing: border-box;
}

.card {
  width: 100%;
  max-width: 520px;
  background: #ffffff;
  border-radius: 16px;
  padding: 30px 25px;
  box-shadow: 0 4px 16px rgba(0, 166, 120, 0.2);
  display: flex;
  flex-direction: column;
}

h2 {
  color: #0ca678;
  text-align: center;
  margin-bottom: 25px;
}

.toggle-show {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
  font-size: 15px;
  color: #495057;
  gap: 8px;
}

.task-list {
  list-style: none;
  padding: 0;
  margin: 0;
  max-height: 300px;
  overflow-y: auto;
}

.task-list li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 14px 0;
  border-bottom: 1px solid #dee2e6;
}

.task-content {
  display: flex;
  align-items: center;
  gap: 10px;
  flex-wrap: wrap;
}

.done {
  text-decoration: line-through;
  color: #adb5bd;
}

.action-buttons {
  display: flex;
  gap: 8px;
}

.edit-btn,
.delete-btn {
  background: none;
  border: none;
  font-size: 18px;
  cursor: pointer;
}

.edit-btn {
  color: #15aabf;
}

.delete-btn {
  color: #fa5252;
}

.input-group {
  display: flex;
  gap: 10px;
  margin-top: 25px;
  flex-wrap: wrap;
}

input[type="text"] {
  flex: 1;
  padding: 12px;
  border: 1px solid #ced4da;
  border-radius: 8px;
  font-size: 15px;
  background-color: #f8f9fa;
  color: #212529;
}

button {
  padding: 10px 16px;
  font-size: 14px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background 0.3s;
  background-color: #0ca678;
  color: white;
}

button:hover {
  background-color: #099268;
}

.cancel-btn {
  background-color: #ffe066;
  color: #212529;
}
</style>
