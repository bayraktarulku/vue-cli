<template>
  <div id="app">
    <div class="ToDo">
      <h1 class="ToDo-Header">Vue ToDo</h1>
      <div class="ToDo-Container">
        <div class="ToDo-Content">
          <ToDoItem v-for="todo in list" :todo="todo" @delete="onDeleteItem" :key="todo.id" />
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
          list: [{id: 1, text: 'Created vue.js project'},
                 {id: 2, text: 'Build to learn'}
                ],
          todo: '',
          active: false,
          logo: Logo,
          'message': []
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
        this.todo = '';
      },
      onDeleteItem(todo){
        this.list = this.list.filter(item => item !== todo);
        this.getMessageData()
        console.log('this.list', this.message)
      },
      getMessageData(){
        getDataAsync('https://jsonplaceholder.typicode.com/posts')
          .then(message => console.log(message));
      }
  },
  created() {}
}

async function getDataAsync(url)
{
  let response = await fetch(url);
  let data = await response.json()
  return data;
}
</script>



