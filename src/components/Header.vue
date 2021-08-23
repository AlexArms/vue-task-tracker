<template>
  <header>
    <h1>{{ title }}</h1>
    <Button
      v-show="homePage"
      @btn-click="$emit('toggle-task-form')"
      v-bind:text="open ? 'Close' : 'Add Task'"
      v-bind:color="open ? 'red' : 'green'"
    />
  </header>
</template>

<script>
import Button from './Button.vue';

export default {
  name: 'Header',
  props: {
    title: String,
    open: Boolean,
  },
  components: {
    Button,
  },
  data() {
    return {
    };
  },
  computed: {
    homePage() {
      if (this.$route.path === '/') {
        return true;
      }
      return false;
    },
  },
};
</script>

<style scoped>
  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
  }
</style>
