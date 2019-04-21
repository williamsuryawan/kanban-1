<template>
    <!-- Modal -->
    <div class="modal fade" :id="'detail-modal-' + category" tabindex="-1" role="dialog" aria-labelledby="detailModalTitle" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered" role="document">
            <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLongTitle">Detail Task: {{ todo.name }} for {{ todo.assignedTo }} Halo</h5>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
                </button>
            </div>
            <div class="modal-body">
                <strong>Task Description:</strong> <br> {{ todo.description}} <br>
                <strong>Point:</strong> <br> {{ todo.point }} <br>
                <strong>Status: </strong> <br> {{ todo.category }} <br>
                
            </div>
            <div class="modal-footer">
                <div class="d-flex justify-content-between">
                    <button class="btn" style="background-color: red; color: white;" v-if="todo.category === 'ToDo'" @click.prevent="updateTodo(todo.id, todo.category, -1)" data-dismiss="modal">Back-Log</button>
                    <button class="btn btn-warning" v-if="todo.category === 'Doing'" @click.prevent="updateTodo(todo.id, todo.category, -1)" data-dismiss="modal">To Do</button>
                    <button class="btn btn-primary" v-if="todo.category === 'Done'" @click.prevent="updateTodo(todo.id, todo.category, -1)" data-dismiss="modal">Doing</button>
                    <button class="btn btn-danger" @click.prevent="deleteTodo(todo.id)">Delete</button>
                    <button class="btn btn-warning" v-if="todo.category === 'Back-Log'" @click.prevent="updateTodo(todo.id, todo.category, 1)" data-dismiss="modal">To Do</button>
                    <button class="btn btn-primary" v-if="todo.category === 'ToDo'" @click.prevent="updateTodo(todo.id, todo.category, 1)" data-dismiss="modal">Doing</button>
                    <button class="btn btn-success" v-if="todo.category === 'Doing'" @click.prevent="updateTodo(todo.id, todo.category, 1)" data-dismiss="modal">Done</button>
                </div>
            </div>
            </div>
        </div>
    </div>
</template>

<script>
import db from '@/api/firebase';
import Swal from 'sweetalert2'

export default {
    props: ['todo', 'category'],
    methods: {
        updateTodo(id, category, direction) {
            console.log("masuk update todo", id, category, direction)
            const categoryList = ['Back-Log', 'ToDo', 'Doing', 'Done']
            db.collection('todos')
                .doc(id)
                .update({
                category: categoryList[categoryList.indexOf(category) + direction]
                })
                .then(() => {
                    console.log('update berhasil')
                    Swal.fire({
                                position: 'center',
                                type: 'success',
                                title: 'You have updated your todo',
                                showConfirmButton: false,
                                timer: 1500
                            })
                })
                .catch(err => {
                    console.log(err)
                    Swal.fire({
                            position: 'center',
                            type: 'error',
                            title: 'You cant update your todo',
                            showConfirmButton: false,
                            timer: 1500
                        })
                })
        },
        deleteTodo(id) {
            console.log("masuk delete todo", id)
            Swal.fire({
                title: 'Are you sure?',
                text: "You won't be able to revert this!",
                type: 'warning',
                showCancelButton: true,
                confirmButtonColor: '#3085d6',
                cancelButtonColor: '#d33',
                confirmButtonText: 'Yes, delete it!'
                })
            .then((result) => {
                if (result.value) {         
                    $('#detail-modal-' + this.category).modal('hide')
                    db.collection('todos')
                        .doc(id)
                        .delete()
                        .then(response => {
                            console.log('delete berhasil', response)
                            Swal.fire({
                                position: 'center',
                                type: 'success',
                                title: 'You have deleted your todo',
                                showConfirmButton: false,
                                timer: 1500
                            })
                        })
                        .catch(err => {
                            console.log(err)
                            Swal.fire({
                                    position: 'center',
                                    type: 'error',
                                    title: 'You cant delete your todo',
                                    showConfirmButton: false,
                                    timer: 1500
                                })
                        })
                }
            })
            
        },
    }
}
</script>
