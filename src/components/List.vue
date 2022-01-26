<template>
    <div class="row">
        <div class="col-12">
            <ul class="list-group mt-3">
                <li v-for="(todo, index) in todos" :key="index" class="list-group-item">
                    <div class="row">
                        <div class="col">
                            <span v-if="todo.isDone"> <!-- jika isDone == true -->
                                <del>
                                    {{ todo.activity }}
                                </del>
                            </span>
                            <span v-else> <!-- jika isDone != true -->
                                {{ todo.activity }}
                            </span>
                        </div>
                        <div class="col-auto">
                            <div class="row gx-2"> <!-- gx => gutters for responsively -->
                                <div class="col">
                                    <div v-if="!todo.isDone">
                                        <button @click="doneTodo(index)" class="btn btn-info">Done</button>
                                    </div>
                                    <div v-else>
                                        <button @click="cancelTodo(index)" class="btn btn-info">Cancel</button>
                                    </div>
                                </div>
                                <div class="col">
                                    <button @click="deleteTodo(index)" class="btn btn-danger">X</button>
                                </div>
                            </div>
                        </div>
                    </div>
                </li>
            </ul>  
        </div>
    </div>
</template>

<script>
export default {
    props: {
        todos: {
            type: Array,
            default: []
        },
    },
    methods: {
        deleteTodo(index) {
            this.$emit('deleteTodo', index) // panggil function deleteTodo dari parent 
        },
        doneTodo(index) {
            this.$emit('doneTodo', index) // panggil function doneTodo dari parent 
        },
        cancelTodo(index) {
            this.$emit('cancelTodo', index) // panggil function cancelTodo dari parent 
        }
    }
}
</script>