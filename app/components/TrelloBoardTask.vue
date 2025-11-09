<template>
  <div
    :title="
      (typeof task.createdAt === 'string'
        ? new Date(task.createdAt)
        : task.createdAt
      ).toLocaleDateString()
    "
    class="task flex gap-3 items-center bg-white rounded-md p-3 mb-3 shadow cursor-pointer hover:bg-gray-50"
    @focus="focused = true"
    @blur="focused = false"
    tabindex="0"
  >
    <DragHandle />
    <span>{{ task.title }}</span>
  </div>
</template>

<script setup lang="ts">
import type { Task, ID } from "~/types";
import DragHandle from "./DragHandle.vue";

const emit = defineEmits<{
  (e: "delete", payload: ID): void;
}>();
const props = defineProps<{
  task: Task;
}>();

const focused = ref(false);

onKeyStroke("Delete", () => {
  if (focused.value) {
    emit("delete", props.task.id);
  }
});
</script>

<style scoped>
.sortable-drag .task {
  transform: rotate(5deg);
}
.sortable-ghost .task {
  position: relative;
}
.sortable-ghost .task::after {
  content: "";
  @apply absolute top-0 left-0 w-full h-full border-2 border-dashed border-gray-400;
}

.task:focus {
  outline: 2px solid #3b82f6; /* Tailwind's blue-500 */
  outline-offset: 2px;
}
</style>
