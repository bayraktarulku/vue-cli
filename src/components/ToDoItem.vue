<template>
    <div class="ToDoItem">
        <p class="ToDoItem-Text">{{ todo.text }}</p>
        <div class="ToDoItem-Delete" @click="deleteItem(todo)">-</div>
        <div class="ToDoItem-Update" @click="updateItem(todo)">*</div>
    </div>
</template>

<script>
    export default {
        name: "to-do-item",
        props: ['todo'],
        methods: {
            deleteItem(todo) {
                this.$emit('delete', todo)
            },
            updateItem(todo){
                this.$emit('update', todo)
            }
        }
    }
</script>
