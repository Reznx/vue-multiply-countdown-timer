<template>
  <li>
    <span>{{ positionNumber }}</span>
    <span v-if="task.eventTime != 'done'">{{ task.eventTime }}</span>
    <span v-else :style="{ color: 'red' }">Событие наступило!</span>
    <span>{{ task.text }}</span>
    <button @click="removeTask">Удалить</button>
  </li>
</template>

<script>
export default {
  props: {
    task: {
      type: Object,
    },
    index: {
      type: Number,
    },
  },
  data() {
    return {};
  },
  computed: {
    positionNumber() {
      return `${this.index + 1}.`;
    },
  },
  methods: {
    removeTask() {
      this.$emit("removeTask", this.index);
    },
  },
};
</script>

<style></style>
