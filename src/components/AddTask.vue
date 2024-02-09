<template>
  <form @submit="addTask">
    <!-- prevent default wasn't used in order to avoid showing the "Please enter a title" message after adding a task -->
    <div class="d-flex flex-row ga-2 mb-3">
      <v-text-field
        v-model="newTask.title"
        label="Add a task"
        placeholder="Title"
        density="comfortable"
        :rules="[v => !!v || 'Please enter a title', v => (v && v.length <= 1024) || 'Title must not exceed 1024 characters']"
      ></v-text-field>
      <div class="d-flex flex-row ga-3">
        <v-switch
          v-model="newTask.status"
          inset
          label="Status"
          color="green"
        ></v-switch>
        <v-btn
          type="submit"
          color="indigo"
          size="large"
        >
          <v-icon icon="mdi-plus-thick" />
        </v-btn>
      </div>
    </div>
  </form>
</template>

<script>
import axios from 'axios';

export default {
  name: 'AddTask',
  data() {
    return {
      newTask: {
        title: '',
        status: false
      }
    };
  },
  methods: {
    async addTask() {
      try {
        if (!this.newTask.title.trim()) {
          throw new Error('Title is required');
        }
        
        await axios.post('http://localhost:5000/tasks', this.newTask);
        this.$emit('task-added');
        this.newTask.title = '';
        this.newTask.status = false;
      } catch (error) {
        console.error('Error adding task:', error.message);
      }
    }
  }
};
</script>

<style scoped>

</style>
