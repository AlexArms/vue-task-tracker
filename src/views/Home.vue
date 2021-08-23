<template>
  <div>
    <div >
      <AddTask
        v-if="showTaskForm"
        v-on:add-task="addTask"
      />
    </div>
    <Tasks
      @toggle-reminder="toggleReminder"
      @delete-task="deleteTask"
      v-bind:tasks="tasks"
    />
  </div>
</template>

<script>
import AddTask from '../components/AddTask.vue';
import Tasks from '../components/Tasks.vue';

export default {
  name: 'Home',
  components: {
    AddTask,
    Tasks,
  },
  data() {
    return {
      tasks: [],
    };
  },
  props: {
    showTaskForm: Boolean,
  },
  methods: {
    /* add a task to json db */
    async addTask(task) {
      try {
        const preTask = await fetch('http://localhost:5000/tasks', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(task),
        });
        const newTask = await preTask.json();
        this.tasks.push(newTask);
      } catch {
        throw new Error('failed to add task');
      }
    },
    /* delete task from json-server */
    async deleteTask(id) {
      // eslint-disable-next-line
      if (confirm('Are you sure?')) {
        try {
          const response = await fetch(`api/tasks/${id}`, {
            method: 'DELETE',
            headers: {
              'Content-Type': 'application/json',
            },
          });
          if (!response.status <= 299 && !response.status >= 200) {
            throw new Error('failed to delete task');
          }
          this.tasks = this.tasks.filter((task) => task.id !== id);
          // response.status === 200 ?
          // this.tasks = this.tasks.filter((task) => task.id !== id)
          // : alert('Failed to delete task');
        } catch (error) {
          throw new Error(error);
        }
      }
    },
    /* toggle reminder status for a task */
    async toggleReminder(task) {
      const updatedTask = { ...task, reminder: !task.reminder };
      try {
        const response = await fetch(`api/tasks/${task.id}`, {
          method: 'PUT',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(updatedTask),
        });
        if (response.status >= 200 && response.status <= 299) {
          for (let i = 0; i < this.tasks.length; i += 1) {
            const tsk = this.tasks[i];
            if (tsk.id === task.id) {
              tsk.reminder = !tsk.reminder;
              break;
            }
          }
        } else {
          throw new Error('failed to toggle reminder');
        }
      } catch (error) {
        throw new Error(error);
      }
    },
    /* get all tasks */
    async fetchTasks() {
      const request = await fetch('api/tasks');
      const tasks = await request.json();
      return tasks;
    },
    /* get a single task */
    async fetchTask(id) {
      const request = await fetch(`api/tasks/${id}`);
      const task = await request.json();
      return task;
    },
  },
  async created() {
    this.tasks = await this.fetchTasks();
  },
};
</script>
