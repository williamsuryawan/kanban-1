<template>
    <div>
        <nav class="navbar navbar-light bg-light">
            <router-link class="navbar-brand" to="/" style="color:black">Kanban by William Suryawan</router-link>
            
            <button type="button" class="btn btn-outline-success float-right" data-toggle="modal" data-target="#newTaskCenter">New Task</button>
        </nav>
        
        <!-- Modal to create new todo -->
        <div class="modal fade" id="newTaskCenter" tabindex="-1" role="dialog" aria-labelledby="newTaskCenterTitle" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalLongTitle">New Task</h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <div class="modal-body">
                        <form>
                            Title:<br>
                            <input type="text" v-model="newTitle" placeholder="Title"><br>
                            Description:<br>
                            <input type="text" v-model="newDesc" placeholder="Description"><br>
                            Point:<br>
                            <input type="text" v-model="newPoint" placeholder="Point"><br>
                            Assigned To: <br>
                            <input type="text" v-model="newPerson" placeholder="Person"><br>
                            <button type="submit" class="btn btn-primary" @click.prevent="addTodo" data-dismiss="modal">Add New Todo</button>
                            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>

</template>

<script>
import db from '@/api/firebase.js'
import Swal from 'sweetalert2'

export default {
  name: 'navbar',
  data() {
    return {
      newTitle:'',
      newDesc: '',
      newPoint: '',
      newPerson: '',
    };
  },
  methods: {
    addTodo() {
      console.log('masuk method add todo')
      db.collection('todos')
        .add({
          name: this.newTitle,
          description: this.newDesc,
          point: this.newPoint,
          assignedTo: this.newPerson,
          category: "Back-Log",
          createdAt: new Date(),
        })
        .then(docref => {
          console.log('berhasil add', docref);
          this.newTitle='';
          this.newDesc='';
          this.newPoint='';
          this.newPerson='';
          Swal.fire({
                  position: 'center',
                  type: 'success',
                  title: 'You have added new todo!',
                  showConfirmButton: false,
                  timer: 1500
              })
        })
        .catch(e => {
          console.log(e);
        });
    },
  },
};
</script>