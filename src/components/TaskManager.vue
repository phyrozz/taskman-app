<template>
  <div class="task-manager">
    <div class="text-right px-10 py-10">
      <h1>Task Manager</h1>
      <small>Created by Joshua Malabanan</small>
    </div>
    <div class="px-10 pb-10 d-flex flex-column ga-5">
      <v-card
        title="My tasks"
        subtitle="Click on the checkbox to toggle the status of the task."
      >
        <div class="px-4 pt-5">
          <v-card v-for="task in tasks" :key="task.id" class="mb-3" :color="task.status ? 'green-lighten-4' : 'indigo-lighten-5'">
            <div class="d-flex flex-row px-3 py-2 align-center justify-space-between">
              <span class="d-inline-block text-truncate">
                <p><b>{{ task.title }}</b></p>
              </span>
              <div class="d-flex flex-row ga-3">
                <v-btn @click="updateTaskStatus(task)" :color="task.status ? 'green-lighten-3' : ''">
                  <v-icon :color="task.status ? 'green' : 'grey'">mdi-check-bold</v-icon>
                </v-btn>
                <v-btn @click="deleteTask(task.id)" color="red-lighten-1">
                  <v-icon icon="mdi-delete" />
                </v-btn>
              </div>
            </div>
          </v-card>
          <AddTask @task-added="getTasks" />
        </div>
      </v-card>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import AddTask from './AddTask.vue';

export default {
  name: 'TaskManager',
  components: {
    AddTask
  },
  data() {
    return {
      tasks: []
    };
  },
  mounted() {
    this.getTasks();
  },
  methods: {
    async getTasks() {
      try {
        const response = await axios.get('http://localhost:5000/tasks');
        this.tasks = response.data;
      } catch (error) {
        console.error('Error fetching tasks:', error);
      }
    },

    async updateTaskStatus(task) {
      try {
        task.status = !task.status;
        await axios.put(`http://localhost:5000/tasks/${task.id}`, { status: task.status });
        this.getTasks();
      } catch (error) {
        console.error('Error updating task status:', error);
      }
    },

    async deleteTask(taskId) {
      try {
        await axios.delete(`http://localhost:5000/tasks/${taskId}`);
        this.getTasks();
      } catch (error) {
        console.error('Error deleting task:', error);
      }
    }
  }
};
</script>

<style scoped>

</style>
