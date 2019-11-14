<template>
    <div class="ToDoItem">
        <p class="ToDoItem-Text">{{todo.text}}</p>
        <div class="ToDoItem-Delete"
             @click="deleteItem(todo)">-
        </div>
    </div>
</template>

<script>
    export default {
        name: "to-do-item",
        props: ['todo'],
        methods: {
            deleteItem(todo) {
                this.$emit('delete', todo)
            }
        }
    }
</script>
