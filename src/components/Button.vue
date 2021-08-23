<template>
  <button class='btn' v-on:click="onClick" v-bind:style="{ background: color }">{{ text }}</button>
</template>

<script>
export default {
  name: 'Button',
  props: {
    text: String,
    color: String,
  },
  methods: {
    onClick() {
      this.$emit('btn-click');
    },
  },
};
</script>

<style>
</style>
