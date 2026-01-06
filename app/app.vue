<script setup>
import { ref } from "vue";

const items = ref([]);
const newItem = ref("");
const editIndex = ref(null);
const editValue = ref("");

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

// START EDIT
const startEdit = (index) => {
  editIndex.value = index;
  editValue.value = items.value[index];
};

// UPDATE
const updateItem = () => {
  if (!editValue.value.trim()) return;
  items.value[editIndex.value] = editValue.value;
  cancelEdit();
};

// CANCEL EDIT
const cancelEdit = () => {
  editIndex.value = null;
  editValue.value = "";
};
</script>

<template>
  <main
    class="max-w-xl mx-auto py-20 px-6 bg-white dark:bg-gray-800 text-black dark:text-white"
  >
    <h1 class="text-3xl font-bold mb-6">CRUD Example</h1>

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
        <!-- VIEW MODE -->
        <span v-if="editIndex !== index">
          {{ item }}
        </span>

        <!-- EDIT MODE -->
        <input
          v-else
          v-model="editValue"
          class="border px-2 py-1 rounded flex-1 mr-2"
        />

        <div class="flex gap-2">
          <button
            v-if="editIndex !== index"
            @click="startEdit(index)"
            class="text-blue-600"
          >
            Edit
          </button>

          <button
            v-if="editIndex === index"
            @click="updateItem"
            class="text-green-600"
          >
            Save
          </button>

          <button
            v-if="editIndex === index"
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
