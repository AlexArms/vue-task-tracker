<template>
  <div v-for="task in tasks" v-bind:key="task.id">
    <Task
      @toggle-reminder="$emit('toggle-reminder', task)"
      @delete-task="$emit('delete-task', task.id)"
      v-bind:task="task"
    />
  </div>
</template>

<script>
import Task from './Task.vue';

export default {
  name: 'Tasks',
  props: {
    tasks: Array,
  },
  components: {
    Task,
  },
  emits: ['delete-task', 'toggle-reminder'],
};
</script>

<style>
</style>
