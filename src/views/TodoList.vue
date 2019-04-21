<template>
    <div class="container">
        <div class="row">
            <div class="col-12 mb-3" v-for="todo in todos" :key="todo.id">
                <div class="card mx-auto"><h2><center><strong>{{ todo.name }}</strong></center></h2>

                    <div class="card-body">
                        Point: {{ todo.point }} <br>
                        Assigned To: {{ todo.assignedTo }}
                        <div class="row" style="padding:5px">
                            <div class="col-sm-12">
                                
                                <!-- Button trigger modal -->
                                <button @click="showDetailModal(todo)" type="button" class="btn btn-primary">
                                    Show Detail
                                </button>
                         
                            </div>

                        </div>
                    </div>
                </div>
            </div>
            
            <!-- <product
                v-for="product in products"
                :product="product"
                :key="product.id" /> -->
        </div>
        <DetailModal :category="category" :todo="selectedTodo" />
    </div>
</template>

<script>
import db from '@/api/firebase';
import DetailModal from '@/components/DetailModal.vue';

export default {
  name: 'todolists',
  props: ['todos', 'category'],
  components: {
      DetailModal
  },
  data() {
      return {
        selectedTodo: {}
      }
  },
  methods: {
    showDetailModal(todo) {
        this.selectedTodo = todo
        $('#detail-modal-' + this.category).modal('show')
    },
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
        })
        .catch(err => {
          console.log(err)
        })
    },
    deleteTodo(id) {
      console.log("masuk delete todo", id)
      db.collection('todos')
        .doc(id)
        .delete()
        .then(response => {
          console.log('delete berhasil', response)
        })
        .catch(err => {
          console.log(err)
        })
    }
  }
};
</script>
