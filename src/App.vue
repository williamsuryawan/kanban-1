<template>
  <div id="app">
    <Navbar />
    <br>
    <div class ="d-flex" style="padding-top:'2px'">
      <div class="col-sm-3">
        
        <div class="card mb-3 bg-danger">
          <h2>Back-Log</h2>
          <div class="card-body">
            <Todolist :todos="backlog" :category="'backlog'" />
          </div>
        </div>
      </div>
      <div class="col-sm-3">
        <div class="card mb-3 bg-warning">
          <h2>To-Do</h2>
          <div class="card-body">
            <Todolist :todos="todo" :category="'todo'" />
          </div>
        </div>
      </div>
      <div class="col-sm-3">
        <div class="card mb-3 bg-primary">
          <h2>Doing</h2>
          <div class="card-body">
            <Todolist :todos="doing" :category="'doing'" />
          </div>
        </div>
      </div>
      <div class="col-sm-3">
        <div class="card mb-3 bg-success">
          <h2>Done</h2>
          <div class="card-body">
            <Todolist :todos="done" :category="'done'" />
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import db from '@/api/firebase.js'
import Navbar from '@/components/Navbar.vue';
import Todolist from '@/views/TodoList.vue';

export default {
  data() {
    return {
      tododata: {},
      backlog: [],
      todo: [],
      doing: [],
      done:[]
    }
  },
  components: {
    Navbar,
    Todolist,
  },
  mounted() {
    db.collection('todos')
      .orderBy('createdAt', 'desc')
      .onSnapshot(querySnapshot => {
        this.backlog=[];
        this.todo=[];
        this.doing=[];
        this.done=[];
        querySnapshot.forEach(doc => {
        let tododata = {};
        // console.log(doc.id);
        // console.log(doc.data());
        tododata = {
          id: doc.id,
          name: doc.data().name,
          description: doc.data().description,
          category: doc.data().category,
          assignedTo: doc.data().assignedTo,
          point: doc.data().point,
          createdAt: doc.data().createdAt
        }
        console.log(tododata)
          if(tododata.category =="Back-Log") {
            this.backlog.push(tododata)
          } else if (tododata.category =="ToDo") {
            this.todo.push(tododata)
          } else if (tododata.category =="Doing") {
            this.doing.push(tododata)
          } else {
            this.done.push(tododata)
          }
        })
        console.log ("hasil push", this.backlog, this.todo)
        console.log("array kosong", this.doing)
      })
  },
  

};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
