<template>
  <div class="container" style="margin-top: 20px;">
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">SIMPLE TODO APP</h5>
        <div class="row">
          <div class="col-10">
            <input type="text" v-model="todo" class="form-control" @keyup.enter="addTodo" autofocus>
          </div>
          <div class="col-2">
            <button class="btn btn-success" @click="addTodo">Add</button>
          </div>
        </div>
         <!-- mengirim data todos sebagai props, dan emit dengan nama deleteTodo-->
        <List :todos="todos" 
          @deleteTodo="deleteTodo" 
          @doneTodo="doneTodo" 
          @cancelTodo="cancelTodo"
        />
        <br>
        <small>Total Todo : {{ totalTodo }} </small>
      </div>
    </div>
  </div>
</template>

<script>
import List from './components/List.vue'

export default {
  components: {
    List
  },
  data() {
    return {
      todo: "",
      todos: []
    }
  },
  mounted() {
    // konversi data string ke JSON
    this.todos = JSON.parse(localStorage.getItem('todos'))
  },
  computed: {
    totalTodo() {
      return this.todos.length
    }
  },
  methods: {
    addTodo() {
      // jika v-model todo tidak kosong
      if (this.todo != "")
      {
        // maka data yang diinput akan ditambahkan dan ditaruh pada posisi index paling awal menggunakan unshift
        this.todos.unshift({
          activity: this.todo,
          isDone: false
        })
        this.todo = ""
        this.saveToLocalStorage()
      }
    },
    deleteTodo(todoIndex) {
      // filter todos menggunakan index
      this.todos = this.todos.filter((item, index) => {
        // jika indexnya tidak sama dengan param todoIndex
        if (index != todoIndex)
        {
          // maka dikembalikan
          return item
        }
      })
      this.saveToLocalStorage()
    },
    doneTodo(todoIndex) {
      // filter todos menggunakan index
      this.todos = this.todos.filter((item, index) => {
        // jika index yang difilter sama dengan param todoIndex dan item.isDone == false
        if (index == todoIndex && item.isDone == false) {
          // maka ubah nilai dari object item.isDone yang awalnya false menjadi true
          item.isDone = true
        }

        // return semua data todo
        return item
      })
      this.saveToLocalStorage()
    },
    cancelTodo(todoIndex) {
      // filter todos menggunakan index
      this.todos = this.todos.filter((item, index) => {
        // jika index yang difilter sama dengan param todoIndex dan item.isDone == true
        if (index == todoIndex && item.isDone == true) {
          // maka ubah nilai dari object item.isDone yang awalnya true menjadi false
          item.isDone = false
        }

        // return semua data todo
        return item
      })
      this.saveToLocalStorage()
    },
    saveToLocalStorage() {
      // parse data JSON ke string, karena localstorage hanya bisa menyimpan string
      localStorage.setItem('todos', JSON.stringify(this.todos))
    }
  }
}
</script>