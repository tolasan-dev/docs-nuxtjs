<script setup>
import { ref } from "vue";

const items = ref([]);
const newItem = ref("");
const editingIndex = ref(null);
const editingValue = ref("");

// CREATE
const addItem = () => {
  if (!newItem.value.trim()) return;
  items.value.push(newItem.value);
  newItem.value = "";
};

// DELETE
const deleteItem = (index) => {
  items.value.splice(index, 1);
};

// EDIT (start)
const startEdit = (index) => {
  editingIndex.value = index;
  editingValue.value = items.value[index];
};

// UPDATE (save)
const updateItem = () => {
  if (!editingValue.value.trim()) return;
  items.value[editingIndex.value] = editingValue.value;
  editingIndex.value = null;
  editingValue.value = "";
};

// CANCEL EDIT
const cancelEdit = () => {
  editingIndex.value = null;
  editingValue.value = "";
};
</script>

<template>
  <main class="max-w-xl mx-auto py-20 px-6 dark:bg-gray-800">
    <h1 class="text-3xl font-bold mb-6">Simple CRUD</h1>

    <!-- CREATE -->
    <div class="flex gap-2 mb-6">
      <input
        v-model="newItem"
        placeholder="Enter item"
        class="flex-1 border px-3 py-2 rounded"
      />
      <button @click="addItem" class="bg-black text-white px-4 rounded">
        Add
      </button>
    </div>

    <!-- READ -->
    <ul class="space-y-3">
      <li
        v-for="(item, index) in items"
        :key="index"
        class="border p-3 rounded flex justify-between items-center"
      >
        <!-- NORMAL VIEW -->
        <span v-if="editingIndex !== index">
          {{ item }}
        </span>

        <!-- EDIT VIEW -->
        <input
          v-else
          v-model="editingValue"
          class="border px-2 py-1 rounded flex-1 mr-2"
        />

        <div class="flex gap-2">
          <button
            v-if="editingIndex !== index"
            @click="startEdit(index)"
            class="text-blue-600"
          >
            Edit
          </button>

          <button
            v-if="editingIndex === index"
            @click="updateItem"
            class="text-green-600"
          >
            Save
          </button>

          <button
            v-if="editingIndex === index"
            @click="cancelEdit"
            class="text-gray-500"
          >
            Cancel
          </button>

          <button @click="deleteItem(index)" class="text-red-600">
            Delete
          </button>
        </div>
      </li>
    </ul>
  </main>
</template>
