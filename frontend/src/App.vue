<script setup lang="ts">
import { ref } from 'vue'

const message = ref('Task');
const newTask = ref('');
const tasks = ref<string[]>([]);
const editingIndex = ref<number | null>(null);
const editingValue = ref('');

function formSubmitted() {
  if (newTask.value.trim()) {
    tasks.value.push(newTask.value);
    newTask.value = '';
  }
}

function deleteTask(idx: number) {
  tasks.value.splice(idx, 1);
  if (editingIndex.value === idx) {
    editingIndex.value = null;
    editingValue.value = '';
  }
}

function editTask(idx: number) {
  editingIndex.value = idx;
  editingValue.value = tasks.value[idx];
}

function saveEdit(idx: number) {
  if (editingValue.value.trim()) {
    tasks.value[idx] = editingValue.value;
  }
  editingIndex.value = null;
  editingValue.value = '';
}
</script>

<template>
  <main>
    <h1 class="main-title">{{ message }}</h1>
    <form @submit.prevent="formSubmitted" class="task-form">
      <label class="input-label">
        <input v-model="newTask" name="newTask" class="task-input" placeholder="What do you need to do?" autocomplete="off" />
      </label>
      <div class="button-container">
        <button class="add-btn" type="submit">➕ Add</button>
      </div>
    </form>

    <h2 class="section-title">Added Tasks</h2>
    <ul class="task-list">
      <li v-for="(task, idx) in tasks" :key="idx" class="task-card">
        <template v-if="editingIndex === idx">
          <input v-model="editingValue" class="edit-input" />
          <button class="save-btn" @click="saveEdit(idx)">💾 Save</button>
          <button class="cancel-btn" @click="editingIndex = null; editingValue = ''">❌ Cancel</button>
        </template>
        <template v-else>
          <span class="task-text">{{ task }}</span>
          <button class="edit-btn" @click="editTask(idx)">✏️ Edit</button>
          <button class="delete-btn" @click="deleteTask(idx)">🗑️ Delete</button>
        </template>
      </li>
    </ul>
  </main>
</template>

<style scoped>
body {
  background: #f5f7fa;
}
main {
  max-width: 500px;
  margin: 2rem auto;
  padding: 2rem 1.5rem;
  background: #fff;
  border-radius: 18px;
  box-shadow: 0 4px 32px rgba(79, 140, 255, 0.08);
}
.main-title {
  text-align: center;
  font-size: 2.2rem;
  font-weight: 700;
  color: #235390;
  margin-bottom: 1.5rem;
  letter-spacing: 1px;
}
.section-title {
  margin-top: 2.5rem;
  font-size: 1.3rem;
  color: #4f8cff;
  letter-spacing: 0.5px;
}
.task-form {
  display: flex;
  gap: 1rem;
  align-items: center;
  margin-bottom: 1.5rem;
  justify-content: center;
}
.input-label {
  flex: 1;
}
.task-input, .edit-input {
  width: 100%;
  padding: 0.7rem 1rem;
  border-radius: 8px;
  border: 1.5px solid #dbeafe;
  font-size: 1.1rem;
  outline: none;
  transition: border 0.2s;
  background: #f8fafc;
}
.task-input:focus, .edit-input:focus {
  border: 1.5px solid #4f8cff;
  background: #fff;
}
.button-container {
  display: flex;
  align-items: center;
}
ul.task-list {
  list-style: none;
  padding: 0;
  margin: 0;
}
.task-card {
  display: flex;
  align-items: center;
  background: #f8fafc;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(79, 140, 255, 0.07);
  padding: 0.7rem 1rem;
  margin-bottom: 1rem;
  gap: 0.5rem;
  transition: box-shadow 0.2s, transform 0.1s;
}
.task-card:hover {
  box-shadow: 0 4px 16px rgba(79, 140, 255, 0.15);
  transform: translateY(-2px) scale(1.01);
}
.task-text {
  flex: 1;
  font-size: 1.08rem;
  color: #22223b;
  word-break: break-word;
}
button {
  min-width: 90px;
  min-height: 38px;
  font-size: 1rem;
  padding: 0.35rem 1.2rem;
  border-radius: 8px;
  border: none;
  color: #fff;
  font-weight: 600;
  box-shadow: 0 2px 8px rgba(79, 140, 255, 0.08);
  transition: background 0.2s, transform 0.1s, box-shadow 0.2s;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 0.3em;
}
.add-btn {
  background: linear-gradient(90deg, #43e97b 0%, #38f9d7 100%);
}
.add-btn:hover, .add-btn:focus {
  background: linear-gradient(90deg, #38f9d7 0%, #43e97b 100%);
}
.edit-btn {
  background: linear-gradient(90deg, #4f8cff 0%, #235390 100%);
}
.edit-btn:hover, .edit-btn:focus {
  background: linear-gradient(90deg, #235390 0%, #4f8cff 100%);
}
.delete-btn {
  background: linear-gradient(90deg, #ff5858 0%, #f09819 100%);
}
.delete-btn:hover, .delete-btn:focus {
  background: linear-gradient(90deg, #f09819 0%, #ff5858 100%);
}
.save-btn {
  background: linear-gradient(90deg, #f7971e 0%, #ffd200 100%);
  color: #333;
}
.save-btn:hover, .save-btn:focus {
  background: linear-gradient(90deg, #ffd200 0%, #f7971e 100%);
  color: #222;
}
.cancel-btn {
  background: linear-gradient(90deg, #bdc3c7 0%, #2c3e50 100%);
}
.cancel-btn:hover, .cancel-btn:focus {
  background: linear-gradient(90deg, #2c3e50 0%, #bdc3c7 100%);
}
</style>
