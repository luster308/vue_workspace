<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<template>
<div id="app">
  <TodoHeader></TodoHeader>
  <TodoInput v-on:addTodo="addTodo"></TodoInput>
  <TodoList v-bind:propsdata="todoItems" @removeTodo="removeTodo"></TodoList>
  <TodoFooter v-on:removeAll="clearAll"></TodoFooter>
</div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoInput from './components/TodoInput.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'
export default {
  data() {
    return {
      todoItems: [],
      todoTime: []
    }
  },
  methods: {
    addTodo(todoItem, todoTime) {
      const url = "http://localhost:8888/Todo/jsp/todowrite.jsp?content="+todoItem+"&time="+todoTime;
      axios.get(url, {xhrFields: {withCredentials: true}})
        .then((response) => {
          console.log(response);
          let tempdata = { "todocontent" : todoItem, "todotime" : todoTime };
          this.todoItems.push(tempdata);
        })
        .catch(function(error) {
          console.log(error);
        })
    },
    clearAll() {
      const url = "http://localhost:8888/Todo/jsp/todoclearAll.jsp";
      axios.get(url, {xhrFields: {withCredentials: true}})
      .then((response) => {
        this.todoItems = [];
      })
      .catch(function(error) {
        console.log(error);
      })
    },
    removeTodo(todoItem, index) {
      const url = "http://localhost:8888/Todo/jsp/todoremove.jsp?deletecontent="+todoItem.todocontent;
      axios.get(url, {xhrFields: {withCredentials: true}})
      .then((response) => {
          this.todoItems.splice(index, 1);
      })
      .catch(function(error) {
        console.log(error);
      })
    }
  }, // methods
  created() {
    const url = "http://localhost:8888/Todo/jsp/todolist.jsp";
    axios.get(url, {xhrFields: {withCredentials: true}})
    .then((response) => {
      for (var i = 0; i < response.data.length; i++) {
        this.todoItems.push(response.data[i]);
      }
    })
  }, // created
  components: {
    'TodoHeader': TodoHeader,
    'TodoInput': TodoInput,
    'TodoList': TodoList,
    'TodoFooter': TodoFooter,
  } // components
} // export default
</script>

<style>
body {
  text-align: center;
  background-color: #F6F6F8;
}

input {
  border-style: groove;
  width: 200px;
}

button {
  border-style: groove;
}

.shadow {
  box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03)
}
</style>
