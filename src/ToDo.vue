<template>
  <div id="app">
    <div class="ToDo">
      <h1 class="ToDo-Header">Vue ToDo</h1>
      <div class="ToDo-Container">
        <div class="ToDo-Content">
          <ToDoItem v-for="todo in list" :todo="todo" @delete="onDeleteItem" @update="onUpdateItem" :key="todo.id" />
        </div>
        <div class="NewToDoItem">
          <input type="text" v-model="todo" v-on:keyup.enter="createNewToDoItem"/>
          <div class="ToDoItem-Add" @click="createNewToDoItem()">+</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ToDoItem from './components/ToDoItem.vue'
import Logo from './static/images/logo.png';
import './static/css/style.css';

export default {
  name: 'to-do',
  components: {
    ToDoItem
  },
  data() {
      return {
          list: [],
          todo: 'Add New To-Do',
          active: false,
          logo: Logo,
          message: []
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
        console.log('Delete', todo)
        fetch('http://localhost:3000/todoItems/' + todo.id, {
          method: 'DELETE',
          headers: {'content-type': 'application/json'},
        })
        this.list = this.list.filter(item => item !== todo);
      },
      onUpdateItem(todo) {
        console.log('Update', todo)
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



