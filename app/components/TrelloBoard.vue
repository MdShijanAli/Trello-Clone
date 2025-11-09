<template>
  <div class="flex gap-4 overflow-x-auto p-4">
    <draggable
      v-model="columns"
      group="columns"
      item-key="id"
      :animation="200"
      handle=".drag-handle"
      class="flex gap-4"
    >
      <template #item="{ element: column }: { element: Column }">
        <div class="column bg-gray-100 rounded-lg p-4 min-w-[250px]">
          <header
            class="text-lg font-semibold mb-4 flex items-center justify-between"
          >
            <input
              class="title-input bg-transparent focus:bg-white rounded px-1 w-4/5"
              @keyup.enter="($event.target as HTMLInputElement).blur()"
              type="text"
              v-model="column.title"
              @keydown.backspace="
                column.title === ''
                  ? (columns = columns.filter((c) => c.id !== column.id))
                  : null
              "
            />
            <DragHandle />
          </header>
          <draggable
            v-model="column.tasks"
            :group="{ name: 'tasks', pull: alt ? 'clone' : true }"
            item-key="id"
            handle=".drag-handle"
            :animation="200"
          >
            <template #item="{ element: task }: { element: Task }">
              <div>
                <TrelloBoardTask
                  :task="task"
                  @delete="
                    column.tasks = column.tasks.filter((t) => t.id !== $event)
                  "
                />
              </div>
            </template>
          </draggable>
          <footer>
            <NewTask @add-task="column.tasks.push($event)" />
          </footer>
        </div>
      </template>
    </draggable>
  </div>
  <div class="flex items-center justify-center mt-5">
    <button
      @click="createColumn"
      class="p-3 bg-blue-500 text-white rounded-md shadow hover:bg-blue-600"
      title="Add Column"
    >
      + Add Column
    </button>
  </div>
</template>

<script setup lang="ts">
import { nanoid } from "nanoid";
import draggable from "vuedraggable";
import TrelloBoardTask from "./TrelloBoardTask.vue";
import type { Column, Task } from "~/types";
import DragHandle from "./DragHandle.vue";
import NewTask from "./NewTask.vue";

const columns = useLocalStorage<Column[]>("columns", [
  {
    id: nanoid(),
    title: "Pending Tasks",
    tasks: [
      {
        id: nanoid(),
        title: "Design Hero Section",
        createdAt: new Date(),
      },
      {
        id: nanoid(),
        title: "Develop Responsive Layout",
        createdAt: new Date(),
      },
    ],
  },
  {
    id: nanoid(),
    title: "In Progress",
    tasks: [
      {
        id: nanoid(),
        title: "Research Target Audience",
        createdAt: new Date(),
      },
      {
        id: nanoid(),
        title: "Create Content Calendar",
        createdAt: new Date(),
      },
    ],
  },
  {
    id: nanoid(),
    title: "Submitted",
    tasks: [
      {
        id: nanoid(),
        title: "Book Venue",
        createdAt: new Date(),
      },
      {
        id: nanoid(),
        title: "Send Invitations",
        createdAt: new Date(),
      },
    ],
  },
  {
    id: nanoid(),
    title: "Completed",
    tasks: [
      {
        id: nanoid(),
        title: "Optimize Meta Tags",
        createdAt: new Date(),
      },
      {
        id: nanoid(),
        title: "Build Backlinks",
        createdAt: new Date(),
      },
    ],
  },
]);

const alt = useKeyModifier("Alt");

const createColumn = () => {
  columns.value.push({
    id: nanoid(),
    title: "New Column",
    tasks: [],
  });
  nextTick(() => {
    (
      document.querySelector(
        ".column:last-of-type .title-input"
      ) as HTMLInputElement
    ).focus();
  });
};
</script>
