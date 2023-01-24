<template>
    <button   @click="clearCompleted">Clear Completed</button>
</template>


<script>
export default {
    name: 'todo-clear-completed',
    props: {
        showClearCompletedButton: {
            type: Boolean,
            required: true,
        }
    },
    methods:{
        clearCompleted(){
            eventBus.$emit('clearCompletedTodos')
        }
    }
}
</script>