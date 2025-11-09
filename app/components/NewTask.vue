<template>
  <div class="new-task">
    <input
      type="text"
      placeholder="Enter task title"
      v-model="taskTitle"
      @focus="focused = true"
      @blur="focused = false"
      @keydown.tab="handleAddTask"
      @keyup.enter="handleAddTask"
      class="w-full p-2 border rounded-md"
    />
  </div>
</template>

<script setup lang="ts">
import { nanoid } from "nanoid";
import type { Task } from "~/types";

const emit = defineEmits<{
  (e: "add-task", payload: Task): void;
}>();

const focused = ref(false);
const taskTitle = ref("");

const handleAddTask = () => {
  if (taskTitle.value.trim()) {
    // Emit the new task title to the parent component
    emit("add-task", {
      title: taskTitle.value,
      createdAt: new Date(),
      id: nanoid(),
    });
    taskTitle.value = "";
  }
};
</script>

<style scoped>
.new-task {
  display: flex;
  gap: 8px;
}
</style>
