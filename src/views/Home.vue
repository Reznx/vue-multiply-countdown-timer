<template>
  <div class="home">
    <task-form @addTask="addTask" />
    <ul class="home__notifications" v-if="tasks.length">
      <task-row
        v-for="(task, index) in tasks"
        :task="task"
        :index="index"
        :key="task.id"
        @removeTask="removeTask"
      />
    </ul>
    <p v-else>Ничего не добавлено в заметки</p>
  </div>
</template>

<script>
import TaskRow from "@/components/TaskRow.vue";
import TaskForm from "@/components/TaskForm.vue";

export default {
  components: { TaskRow, TaskForm },
  name: "Home",
  data() {
    return {
      tasks: [],
      interval: null,
    };
  },
  methods: {
    idGenerator() {
      return (Date.now() - Math.random() * 16).toString(16);
    },
    addTask({ text, date }) {
      this.tasks.push({
        text: text,
        eventTime: "",
        time: new Date(date).getTime(),
        id: this.idGenerator(),
      });
      this.updateTimer();
      this.updateStorage();
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
      this.updateStorage();
    },
    updateStorage() {
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    updateTimer() {
      this.interval = setInterval(() => {
        for (let key in this.tasks) {
          let taskTime = parseInt(this.tasks[key]["time"]);
          let nowTime = new Date().getTime();
          let rightTime = taskTime - nowTime;
          let zeroTime = Math.floor(rightTime / 1000 - 60);

          if (zeroTime > 0) {
            let dd = Math.floor(rightTime / 1000 / 60 / 60 / 24);
            let hh = Math.floor((rightTime / 1000 / 60 / 60) % 24);
            let mm = Math.floor((rightTime / 1000 / 60) % 60);
            this.tasks[key]["eventTime"] =
              dd + " д " + hh + " ч " + mm + " мин ";
          } else {
            this.tasks[key]["eventTime"] = "done";
          }
        }
      }, 1000);
    },
  },
  created() {
    const tasks = JSON.parse(localStorage.getItem("tasks"));
    if (tasks) {
      this.tasks = tasks;
    }
  },
  mounted() {
    this.updateTimer();
  },
  beforeDestroy() {
    clearInterval(this.interval);
  },
};
</script>
