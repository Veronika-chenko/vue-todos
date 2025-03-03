<script setup>
import { Icon } from "@iconify/vue";
const props = defineProps({
  todo: {
    type: Object,
    required: true,
  },
});
defineEmits(['toggle-complete', 'toggle-edit', 'update-todo', 'delete-todo'])
</script>

<template>
  <li>
    <input
      type="checkbox"
      :checked="todo.isCompleted"
      @input="$emit('toggle-complete', todo.id)" />
    <div class="todo">
      <input
        v-if="todo.isEditing"
        type="text"
        :value="todo.todo"
        @input="$emit('update-todo', $event.target.value, todo.id)" />
      <span v-else :class="{ 'completed-todo': todo.isCompleted }">
        {{ todo.todo }}
      </span>
    </div>
    <div class="todo-actions">
      <Icon
        v-if="todo.isEditing"
        icon="ph:check-circle"
        class="icon"
        color="#41b080"
        width="22px"
        height="22px"
        @click="$emit('toggle-edit', todo.id)" />
      <Icon
        v-else
        icon="ph:pencil-fill"
        class="icon"
        color="#41b080"
        width="22px"
        height="22px"
        @click="$emit('toggle-edit', todo.id)" />
      <Icon
        icon="ph:trash"
        class="icon"
        color="#f95e5e"
        width="22px"
        height="22px"
        @click="$emit('delete-todo', todo.id)" />
    </div>
  </li>
</template>

<style lang="scss" scoped>
li {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 16px 10px;
  background-color: #f1f1f1;
  box-shadow: 0 20px 25px -5px rgb(0 0 0 / 0.1),
    0 8px 10px -6px rgb(0 0 0 / 0.1);

  input[type="checkbox"] {
    flex-shrink: 0;
    appearance: none;
    width: 20px;
    height: 20px;
    background-color: #fff;
    border-radius: 50%;
    box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
    cursor: pointer;

    &:checked {
      background-color: #41b080;
    }
  }

  .todo {
    flex: 1;
    width: calc(100% - 110px);
    overflow: hidden;

    .completed-todo {
      text-decoration: line-through;
    }

    input[type="text"] {
      width: 100%;
      padding: 2px 6px;
      border: 2px solid #41b080;
      font-size: 16px;
    }
  }

  .todo-actions {
    display: flex;
    flex-shrink: 0;
    gap: 6px;
    .icon {
      cursor: pointer;
    }
  }
}
</style>
