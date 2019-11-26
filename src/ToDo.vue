<template>
  <div id="app">
    <div v-if="selected == 'ToDoContainer'" class="ToDo">
      <button @click="selected='TrashContainer';">Back</button>
      <h1 class="ToDo-Header">Vue ToDo</h1>
      <div class="ToDo-Container">
        <div class="ToDo-Content">
          <ToDoItem v-for="todo in list" :todo="todo" @delete="onDeleteItem" @update="onUpdateItem" :key="todo.id" />
        </div>
        <div class="NewToDoItem">
          <input type="text" placeholder="Add New To-Do" v-model="todo" v-on:keyup.enter="createNewToDoItem"/>
          <div class="ToDoItem-Add" @click="createNewToDoItem()">+</div>
        </div>
      </div>
    </div>
    <div v-else-if="selected == 'TrashContainer'" class="Trash">
      <button @click="selected='ToDoContainer';">ToDo</button>
       <h1 class="ToDo-Header">Vue Trash</h1>
       <div class="NumberItem">
         <input type="text" placeholder="Add Number" v-model="number"/>
       </div>
    </div>
  </div>
</template>

<script>
import ToDoItem from './components/ToDoItem.vue'
import './static/css/style.css';

export default {
  name: 'to-do',
  components: {
    ToDoItem
  },
  data() {
      return {
          list: [],
          todo: '',
          showModal: false,
          active: false,
          message: [],
          number: null,
          selected: 'ToDoContainer',
      }
  },
  methods: {
      createNewToDoItem() {
        if (!this.todo){
          alert("Enter a todo!");
          return
        }
        const newId = Math.max.apply(null, this.list.map(t => t.id)) + 1;
        this.list.push({ id: newId, text: this.todo});
        fetch('http://localhost:3000/todoItems', {
          method: 'post',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({ id: newId, text: this.todo})
        }).then(res=>res.json())
        this.todo = '';
      },
      onDeleteItem(todo){
        fetch('http://localhost:3000/todoItems/' + todo.id, {
          method: 'DELETE',
          headers: {'content-type': 'application/json'},
        })
        this.list = this.list.filter(item => item !== todo);
      },
      onUpdateItem(todo) {
        this.showModal = true
      },
      getMessageDatacallback(resp) {
        this.message= resp
        this.list= resp
      },
      getMessageData(){
        getDataAsync('http://localhost:3000/todoItems')
        .then(this.getMessageDatacallback);
      },
  },
  created() {
    this.getMessageData()
  }
}
// Get Data
async function getDataAsync(url)
{
  let response = await fetch(url);
  let data = await response.json()
  return data;
}
</script>
