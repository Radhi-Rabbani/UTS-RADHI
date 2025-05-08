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
</template>

<style>
</style>
