<template>
  <AddTask @add-task="addTask" v-show="showAddTask" />
  <Tasks
    :tasks="tasks"
    @delete-task="deleteTask"
    @toggle-reminder="toggleReminder"
  />
</template>

<script>
import Tasks from "../components/Tasks";
import AddTask from "../components/AddTask.vue";

export default {
  name: "Home",
  components: {
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [],
    };
  },
  props: {
    showAddTask: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    async deleteTask(id) {
      if (confirm("Are you sure?")) {
        const res = await fetch(`api/tasks/${id}`, {
          method: "DELETE",
        });

        if (res.status === 200) {
          this.tasks = this.tasks.filter((x) => x.id !== id);
        }
      }
    },
    async toggleReminder(id) {
      const taskToUpdate = this.tasks.find((x) => x.id === id);
      const updatedTask = { ...taskToUpdate, reminder: !taskToUpdate.reminder };
      const res = await fetch(`api/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(updatedTask),
      });

      const data = await res.json();

      this.tasks = this.tasks.map((x) => {
        if (x.id === id) {
          return data;
        }

        return x;
      });
    },
    async addTask(task) {
      const res = await fetch("api/tasks", {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(task),
      });

      const data = await res.json();

      this.tasks = [...this.tasks, data];
    },
    async fetchTasks() {
      const res = await fetch("api/tasks");
      const data = await res.json();

      return data;
    },
    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`);
      const data = await res.json();

      return data;
    },
  },
  async created() {
    this.tasks = await this.fetchTasks();
  },
};
</script>