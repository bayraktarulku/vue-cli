<template>
  <div id="app">
    <div class="ToDo">
      <h1 class="ToDo-Header">Vue ToDo</h1>
      <div class="ToDo-Container">
        <div class="ToDo-Content">
          <ToDoItem v-for="todo in list"
                    :todo="todo"
                    @delete="onDeleteItem"
                    :key="todo.id" />
        </div>
        <div class="NewToDoItem">
            <input type="text" v-model="todo" v-on:keyup.enter="createNewToDoItem"/>
            <div class="ToDoItem-Add"@click="createNewToDoItem()">+
            </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ToDoItem from './components/ToDoItem.vue'
import Logo from './assets/logo.png';

export default {
  name: 'to-do',
  components: {
    ToDoItem
  },
  data() {
      return {
          list: [
              {
                id: 1,
                text: 'Created vue.js project'
              },
              {
                id: 2,
                text: 'Build to learn'
              }
          ],
          todo: '',
          logo: Logo
      }
  },

  methods: {
      createNewToDoItem() {
        //validate todo
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
      }

  },
}
</script>

<style>

  body {
    margin: 0;
    padding: 0;
    font-family: -apple-system, BlinkMacSystemFont, Helvetica Neue, Helvetica, Arial, sans-serif;
    background: linear-gradient(#575a57, #232425);
    height: auto;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .ToDo {
    text-align: center;
    border: 1px solid white;
    width: 80vw;
    height: auto;
    box-shadow: 2px 3px 15px rgba(0, 0, 0, 0.5);
    background: #f6f6f6;
    padding-bottom: 60px;
    margin: 40px auto;
  }

  .ToDo-Header {
    color: black;
    font-family: -apple-system, BlinkMacSystemFont, Helvetica Neue, Helvetica, Arial, sans-serif;
    font-weight: 400;
    text-transform: uppercase;
    margin: 70px auto 30px;
  }

  .ToDo-Add {
    width: 20px;
    padding: 5px;
    height: 20px;
    cursor: pointer;
    background: #329bce;
    border-radius: 10px;
    box-shadow: 1px 1px 1px #c70202;
    color: white;
    font-size: 18px;
    margin-right: 5px;
  }

  .ToDo-Add:hover {
    box-shadow: none;
    margin-top: 21px;
    margin-left: calc(auto + 1px);
  }

  .ToDo-Container {
    width: 80%;
    margin: 0 auto;
  }
  .ToDoItem-Add {
      width: 20px;
      padding: 5px;
      height: 20px;
      cursor: pointer;
      background: #1eaaec;
      border-radius: 10px;
      box-shadow: 1px 1px 1px #1eaaec;
      color: white;
      font-size: 18px;
      margin-right: 16px;
  }
  .NewToDoItem {
      display: flex;
      justify-content: center;
      align-items: center;
  }

  input {
    text-align: center;
    width: 91%;
    padding: 8px;
    padding-left: 4px;
    font-size: 1em;
    margin: 10px auto;
    box-shadow: 1px 3px 20px 0px rgba(0, 0, 0, 0.3)
  }
</style>
