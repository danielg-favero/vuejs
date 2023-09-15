<template>
  <TheHeader @mouseover="onClick()">Hello World</TheHeader>
  <div><button @click="onClick()">
    Enviar
  </button></div>

  <h1>Uncompleted Todos</h1>
  <div v-for="todo in uncompletedTodos" :key="todo.id">
    <input type="checkbox" v-model="todo.completed">
    {{ todo.title }}
  </div>
  
  <h1>Completed Todos</h1>
  <div v-for="todo in completedTodos" :key="todo.id">
    <input type="checkbox" v-model="todo.completed">
    {{ todo.title }}
  </div>

  <input type="text" v-model="name">

  {{ name }}

</template>

<script>
import TheHeader from './components/TheHeader.vue'

export default {
  name: 'App',

  // Componentes que serão usados no componente
  components: {
    TheHeader,
  },

  methods: {
    onClick() {
      console.log('click')
    }
  },

  // O VueJS vai cachear esses métodos e irá computador novamente se os valores dentro mudarem
  computed: {
    fullName() {
      return `${this.user.firstName} ${this.user.lastName}`
    },
    uncompletedTodos() {
      return this.todos.filter(todo => !todo.completed)
    },
    completedTodos() {
      return this.todos.filter(todo => todo.completed)
    }
  },

  watch: {
    name(newValue, oldValue) {
      console.log(newValue, oldValue)
    }
  }, 
  
  // Dados usados no componente
  data() {
    return {
      showHeader: true,
      name: 'Paulo',
      user: {
        firstName: 'Daniel',
        lastName: 'Favero'
      },
      sports: '',
      todos: [
        {
          userId: 1,
          id: 1,
          title: "delectus aut autem",
          completed: false
        },
        {
          userId: 1,
          id: 2,
          title: "quis ut nam facilis et officia qui",
          completed: false
        },
        {
          userId: 1,
          id: 3,
          title: "fugiat veniam minus",
          completed: false
        },
        {
          userId: 1,
          id: 4,
          title: "et porro tempora",
          completed: true
        },
        {
          userId: 1,
          id: 5,
          title: "laboriosam mollitia et enim quasi adipisci quia provident illum",
          completed: false
        }
      ]
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.title {
  font-size: 20px;
  color: blue;
}
</style>
