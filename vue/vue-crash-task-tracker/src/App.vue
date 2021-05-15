<template>
  <div class="container">
    <h1>
      <Header 
      @toggle-add-task="toggleAddTask" 
      title="Task Tracker"
      :showAddTask="showAddTask" 
      />
      </h1>
    <div v-if="showAddTask">
      <AddTask @add-task="addTask" />
    </div>
    <Tasks
      @delete-task="deleteTask"
      @toggle-reminder="toggleReminder"
      :tasks="tasks"
    />
  </div>
</template>

<script>
import Header from "./components/Header";
import AddTask from "./components/AddTask";
import Tasks from "./components/Tasks";
export default {
  name: "App",
  components: {
    Header,
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [],
      showAddTask: false,
    };
  },
  methods: {
    toggleAddTask() {
      this.showAddTask = !this.showAddTask;
    },
  async addTask(task) {
    const response = await fetch('http://localhost:5000/tasks', { 
      method: 'POST',
      headers: {
        'Content-type': 'application/json',
      },
       body: JSON.stringify(task)
    })

      const data = await res.json()

      this.tasks = [...this.tasks, data];
    },
    deleteTask(id) {
      if (confirm("Are you sure?")) {
        this.tasks = this.tasks.filter((task) => task.id !== id);
      }
    },
    toggleReminder(id) {
      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: !task.reminder } : task
      );
    },
    async fetchTasks() {
      try {
      const response = await fetch('http://localhost:5000/tasks')
      const data = await response.json()
      return data
      }
      catch(err) {
        console.log(`couldnt fetch : ${err}`)
      }
      
    },
    async fetchTask() {
      try {
      const response = await fetch(`http://localhost:5000/tasks/${id}`)
      const data = await response.json()
      return data
      }
      catch(err) {
        console.log(`couldnt fetch : ${err}`)
      }
      
    },
  },
 async created() {
    this.tasks = await this.fetchTasks()
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: "Poppins", sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
