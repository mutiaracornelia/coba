<template>
  <div class="container mt-20">
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">Simple</h5>
        <div class="row">
          <div class="col-10">
            <input v-model="todo" type="text" class="form-control" @keyup.enter="add">
          </div>
          <div class="col-2">
            <button @click="add" class="btn btn-success">Add</button>
          </div>
        </div>
        <list :todos="todos" @deleteTodo="deleteTodo" @doneTodo="doneTodo"/>
        <small>Total : {{ totalTODO }}</small>
      </div>
    </div>

  </div>
</template>

<script>
import List from './components/List.vue';
export default {
  components: { List },
  data(){
    return{
      todo: "",
      todos: []
    }
  },      
  mounted(){
      this.todos = JSON.parse(localStorage.getItem('todos'));
  },
  computed: {
    totalTODO() {
      return this.todos.length;
    }
  },
  methods:{
    add(){
      this.todos.unshift({
        activity: this.todo,
        isDone: false,
      });
      this.todo="";
      this.saveToLocalStorage();
    },
    deleteTodo(todoIndex){
      this.todos = this.todos.filter((item,index) => {
        if (index != todoIndex) {
          return item
        }
      });
      this.saveToLocalStorage();
    },
    doneTodo(todoIndex){
      this.todos = this.todos.filter((item, index) => {
        if(index == todoIndex){
          item.isDone = true;
        }
        return item;
      });
      this.saveToLocalStorage();
    },
    saveToLocalStorage(){
      localStorage.setItem('todos', JSON.stringify(this.todos));
    }
  }
}
</script>