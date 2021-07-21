<template>
  <button @click="onClick()" :style="{ background: color }" class="btn">
    {{ text }}
  </button>
</template>

<script>
export default {
  name: "Button",
  props: {
    text: {
      type: String,
      default: "Add Task",
    },
    color: {
      type: String,
      default: "green",
    },
  },
  methods: {
      onClick() {
          this.$emit('btn-click')
      }
  },
  emits: ['toggle-add-task', 'btn-click']
};
</script>ß