<template>
    <div class="todo-item">
        <div class="todo-item-left">
            <input type="checkbox" v-model="completed" @change="doneEdit">
            <div v-if="!edited" @dblclick="editTodo" class="todo-item-label" :class="{ completed: completed }">
                {{ title }}
            </div>
            <input v-else type="text" class="todo-item-edit" v-model="title" @keyup.esc="cancelEdit" v-focus
                @blur="doneEdit" @keyup.enter="doneEdit">
        </div>
        <div class="remove-item" @click="removeTodo(index)">
            &times;
        </div>
    </div>



</template>


<script>
export default {
    name: 'todo-item',
    props: {
        todo: {
            type: Object,
            required: true,
        },

        index: {
            type: Number,
            required: true,
        },
        checkAll: {
            type: Boolean,
            required: true
        }
    },
    data() {
        return {
            'id': this.todo.id,
            'title': this.todo.title,
            'completed': this.todo.completed,
            'edited': this.todo.edited,
            'beforeEditCache': '',
        }
    },
    watch: {
        checkAll() {
            if (this.checkAll) {
                this.completed = true;
            }
            else {
                this.completed = this.todo.completed;
            }
        }
    },
    directives: {
        focus: {
            inserted: function (el) {
                el.focus();
            }
        }
    },
    methods: {
        removeTodo(index) {
            eventBus.$emit('removedTodo', index)
        },
        editTodo() {
            this.beforeEditCache = this.title
            this.edited = true
        },
        doneEdit() {
            this.edited = false
            eventBus.$emit('finishedEdit', {
                'index': this.index,
                'todo': {
                    'id': this.id,
                    'title': this.title,
                    'completed': this.completed,
                    'edited': this.edited
                }
            })
        },
        cancelEdit() {
            this.title = this.beforeEditCache
            this.edited = false
        },
    }
}

</script>


<style>

</style>