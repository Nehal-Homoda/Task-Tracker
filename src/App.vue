<template>
  <div class="container">
    <Header @toggle-add-task="toggleAddTask" title="Task Tracker" :showAddTask="showAddTask" />
    <div v-if="showAddTask" >
       <AddTask @add-task='addTask'/>
    </div>
   
    <Tasks @toggle-reminder="toggleReminder()" @delete-task="deleteTask" :tasks="task"/>
  </div>
</template>
<script>
import Header from "./components/Header.vue";
import Tasks from "./components/Tasks.vue";
import AddTask from "./components/AddTask.vue"
export default {
  name: "App",
  components: {
    Header,
    Tasks,
    AddTask,
  },
  data() {
    return {
      task: [],
      showAddTask:false
    };
  },
  methods:{
    deleteTask(id){
      if(confirm('Are you sure?'))
      this.task=this.task.filter((task)=>task.id !==id)
    },
    toggleReminder(id){
      this.task=this.task.map((task)=>
      task.id===id ? {...task,reminder:!task.reminder}:task)

    },
    async addTask(task){
      const res=await fetch("http://localhost:5000/task",{
        method:'POST',
        headers:{
          'content-type':'application/json',
         
        },
         body:JSON.stringify(task)
      })
      const data=await res.json()
      this.task=[...this.task,data]

    },
    toggleAddTask(){
      this.showAddTask=!this.showAddTask;
    },
    async fetchTasks(){
      const res=await fetch('http://localhost:5000/task')
      const data=await res.json()
      return data
    },
      async fetchTasks(id){
      const res=await fetch(`http://localhost:5000/task/${id}`)
      const data=await res.json()
      return data
    }
  }
  ,
 async created() {
    this.task =await this.fetchTasks()
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
