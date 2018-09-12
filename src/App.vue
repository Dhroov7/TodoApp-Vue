<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>{{ msg }}</h1>
    <input id="input-box" placeholder="Enter you want to add to your todo list" v-model="newTodo"/>
    <button v-on:click="addTodo">Add todo</button>
    <ul>
      <li v-for="item in todos" v-bind:class="{'completed' : item.done}">
        {{ item.title }}
        <input type="checkbox" v-on:change="completeTodo" v-model="item.done"/>
      </li>
    </ul>
    <button v-on:click="removeTodos">Clear Todos</button>
  </div>
</template>

<script>
var id = 3
import axios from 'axios';
const JSONAPIDeserializer = require('jsonapi-serializer').Deserializer;

export default {
  name: 'app',
  data () {
    return {
      msg: 'Todo App Using Vue.js',
      newTodo: '',
      todos: []
    }
  },
  methods: {
    addTodo: function () {
      var value = this.newTodo
      if(!value) {
        return
      }
      this.todos.push({
        id:id,
        text: value,
        done:false
      })

      id++

      this.newTodo = ''
    },
    removeTodos: function () {
      this.todos = []
    },
    getTodos: function () {
      var todosList = []
      axios.get('http://localhost:3232/api/todos').then( (response) => {
        new JSONAPIDeserializer().deserialize(response.data, function (err,data) {
          data.map(i => todosList.push(i))
        })
      })
      this.todos = todosList
    }
  },
  beforeMount(){
    this.getTodos()
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

#input-box{
  width: 40%;
}

.completed{
  color: mediumseagreen;
}
</style>
