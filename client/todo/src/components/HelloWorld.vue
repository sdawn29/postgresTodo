<template>
<div class="md:w-1/3 mx-auto px-4 bg-white py-2 rounded-lg my-4 shadow-lg">
  <div class="text-3xl my-4 nav">TODOS</div>
  <div class="flex justify-between">
    <div class="w-3/4 flex items-center mr-2 ">
      <input type="text" class="p-2 border rounded border-black w-full shadow-inner" placeholder="Enter todo to add" v-model="newTodo">
    </div>
    <div class="flex items-center w-1/4 ml-2">
      <button class="bg-green-200 text-green-800 w-full hover:bg-green-300 hover:text-green-800 p-2 font-semibold rounded" @click="addTodo()">Add Todo</button>
    </div>
  </div>
  <div class="mt-6 mb-4 font-bold border-b pb-2">
    <div>
      All Todos
    </div>
  </div>
  <div v-for="todo in todos" :key="todo.todo_id" class="bg-gray-100 my-2 p-4 rounded-md hover:shadow-md font-semibold text-gray-700 flex justify-between border-l-4 hover:border-blue-600">
    <div class="flex items-center w-3/4">
      <div class="flex-wrap ">
        {{todo.description}}
      </div>
    </div>
    <div class="flex items-center">
      <div class="flex items-center p-1 mr-2 cursor-pointer hover:text-blue-600">
        <i class="fas fa-edit"></i>
      </div>
      <div class="flex items-center p-1 cursor-pointer hover:text-red-600" @click="deleteTodo(todo.todo_id)">
        <i class="fas fa-trash-alt"></i>
      </div>
    </div>
  </div>
</div>
</template>

<script>
import axios from "axios";
export default {
  mounted() {
    this.getTodos();
  },
  data() {
    return {
      newTodo: "",
      todos: "",
    }
  },
  methods: {
    async getTodos() {
      const config = {
        method: "get",
        url: "http://localhost:5000/todos"
      };
      try {
        const response = await axios(config);
        this.todos = response.data;
      } catch (error) {
        // console.log(error);
      }
    },
    async addTodo() {
      const config = {
        method: "post",
        url: "http://localhost:5000/todos",
        data: { "description": this.newTodo }
      };
      try {
        await axios(config);
        console.log("Success Added new Todo")
        this.newTodo = "";
        this.getTodos();
      } catch (error) {
        console.log(error);
      }
    },
    async deleteTodo(id) {
      const config = {
        method: "delete",
        url: "http://localhost:5000/todos/"+id
      };
      try {
        await axios(config);
        this.getTodos();
      } catch (error) {
        console.log(error);
      }
    },
  },
}
</script>

<style>

</style>