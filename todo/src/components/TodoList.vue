<template>
    <div id="center">
        <h2>this is todo-list</h2>
        <input type="text " @keyup.enter="addTodo" class="todo-input" placeholder="task that you want to accomplish!"
            v-model="newTodo">
        <todo-item v-for=" (todo, index) in todosFiltered" :key="todo.id" :todo="todo" :index="index" :checkAll="!anyRemaining">
            
        </todo-item>

        <div class="extra-container">
            <todo-check-all :anyRemaining="anyRemaining"></todo-check-all>
           <todo-item-remaining :remaining="remaining"></todo-item-remaining>
        </div>

        <div class="extra-container">
            <todo-filtered></todo-filtered>
            <div>
                <todo-clear-completed :showClearCompletedButton="showClearCompletedButton"></todo-clear-completed>
                
            </div>
        </div>
    </div>

</template>

<script>
import TodoItem from './TodoItem.vue'
import TodoItemRemaining from './TodoItemRemaining.vue'
import TodoCheckAll from './TodoCheckAll.vue'
import TodoFiltered from './TodoFiltered.vue'
import TodoClearCompleted from './TodoClearCompleted.vue'
export default {
    name: 'todo-list',

    components: {
        TodoItem,
        TodoItemRemaining,
        TodoCheckAll,
        TodoFiltered,
        TodoClearCompleted,
    },
    data() {
        return {
            newTodo: '',
            idForTodo: 3,
            beforeEditCache: '',
            filter: 'all',
            todos: [
                {
                    'id': 1,
                    'title': 'complete first video of todo-list',
                    'completed': false,
                    'edited': false,
                },
                {
                    'id': 2,
                    'title': 'complete second video of todo-list',
                    'completed': false,
                    'edited': false,
                },
                
            ]
        }
    },
   created(){
    eventBus.$on('removedTodo',(index)=> this.removeTodo(index))
    eventBus.$on('finishedEdit',(data)=> this.finishedEdit(data))
    eventBus.$on('checkAllChanged',(checked)=> this.checkAllTodos(checked))
    eventBus.$on('filterChanged',(filter)=> this.filter = filter)
    eventBus.$on('clearCompletedTodos', ()=> this.clearCompleted())
},
    computed: {
        remaining() {
            return this.todos.filter(todo => !todo.completed).length
        },
        anyRemaining() {
            return this.remaining != 0
        },
        todosFiltered() {
            if (this.filter == 'all') {
                return this.todos
            } 
            else if (this.filter == 'active') {
                return this.todos.filter(todo => !todo.completed)
            }
            else if (this.filter == 'completed') {
                return this.todos.filter(todo => todo.completed)
            }
        },
        showClearCompleted() {
            return this.todos.filter(todo => todo.completed).length>0
        }
    },
    methods: {
        addTodo() {
             if(this.newTodo.trim().length==0){
                alert("cant insert! empty todo list")
                return 
             }
            // alert('adding a new todo-list')
            this.todos.push({
                id: this.idForTodo,
                title: this.newTodo,
                completed: false
            })
            this.newTodo = ''
            this.idForTodo++;
        },
        removeTodo(index) {
            this.todos.splice(index, 1) 
        },
        directives: {
        focus: {
            inserted: function (el) {
                el.focus();
            }
        }
    },
        
        editTodo(todo) {
            this.beforeEditCache = todo.title
            todo.edited = true
        },
        doneEdit(todo) {
            todo.edited = false
        },
        cancelEdit(todo) {
            todo.title = this.beforeEditCache
            todo.edited = false
        },
        checkAllTodos() {
            this.todos.forEach((todo) =>
                todo.completed = event.target.checked)
            
        },
        clearCompleted() {
            this.todos = this.todos.filter(todo => !todo.completed)
        },
        finishedEdit(data) {
            this.todos.splice(data.index,1,data.todo)
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style >
.center{
    display: flex;
    align-items: center;
    justify-content: center;
}
.todo-input {
    width: 80%;
    padding: 10px 18px;
    font-size: 18px;
    margin-bottom: 16px;
}

.todo-item {
    margin-bottom: 12px;
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.remove-item:hover {
    cursor: pointer;
    margin-left: 14px;
    color: black;

}

.completed {
    text-decoration: line-through;
    color: grey;
}

.extra-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 16px;
    border-top: 2px solid grey;
    padding-top: 14px;
    margin-bottom: 14px;
}

button {
    font-size: 14px;
    background-color: white;
    appearance: none;


}

button:hover {
    background-color: lightgreen;
}

button:focus {
    outline: none;
}

.active {
    background: lightgreen;
}
.fade-enter-active {
    transition: opacity .3s;
}
.fade-enter, .fade-leave-to {
    opacity: 0;
}
</style>
