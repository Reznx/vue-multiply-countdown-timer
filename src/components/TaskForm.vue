<template>
  <form @submit.prevent="addTask">
    <div class="home__block">
      <div class="ui__input">
        <label for="text">Текст</label>
        <input
          id="text"
          v-model="text"
          placeholder="Введите текст заметки"
          type="text"
        />
      </div>
      <div class="ui__input">
        <label for="datetime">Время</label>
        <input id="datetime" v-model="date" type="datetime-local" />
      </div>
      <button>Создать событие</button>
    </div>

    <div v-show="error" :style="{ color: 'red' }">
      <span>Please Enter your task name and date</span>
    </div>
  </form>
</template>

<script>
export default {
  data() {
    return {
      text: "",
      date: "",
      error: false,
    };
  },
  methods: {
    addTask() {
      if (this.text !== "" && this.date !== "") {
        this.$emit("addTask", { text: this.text, date: this.date });
        this.text = "";
        this.date = "";
        this.error = false;
      } else {
        this.error = true;
      }
    },
  },
};
</script>

<style></style>
