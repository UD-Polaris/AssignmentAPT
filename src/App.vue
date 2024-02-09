<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">Welcome to your work of the day..! <strong><input type="text" placeholder="Your Name" v-model="name" /></strong></h2>
    </section>

    <!-- create to-do section -->
    <section class="create-todo">
      <!-- heading for creating a to-do list -->
      <h3 class="create-todo-heading">Create Your To-Do List</h3>
      
      <form @submit.prevent="addTodo">
        <div class="input-container">
          <label>Add :</label>
          <input type="text" placeholder="Complete Apt coder task...." v-model="input_content"/>
        </div>

        <!-- button to add the to-do  -->
        <button type="submit" class="add-todo-button">Add Todo</button>
      </form>
    </section>

    <!-- to-do list section -->
    <section class="todo-list">


      <!-- heading for the to-do list -->
      <h3 class="todo-list-heading">To-Do List</h3>


      <!-- list of to-do items -->
      <div class="list">
        <!-- loop through and display each to-do item -->
        <div
          v-for="todo in sortedTodos"
          :key="todo.createdAt"
          :class="`todo-item ${todo.done && 'done'}`"
        >
          <!-- checkbox for marking the to-do item as done -->
          <label>
            <input type="checkbox" v-model="todo.done" class="checkbox" />
            <!-- Colored bubble indicating the category -->
            <span :class="`bubble ${todo.category}`"></span>
          </label>
          <!-- input for editing the to-do item content -->
          <div class="todo-content">
            <input type="text" v-model="todo.content"/>
            <!-- display creation date -->
            <small>Created: {{ new Date(todo.createdAt).toLocaleString() }}</small>
          </div>
          <!-- button to delete the to-do item -->
          <div class="actions">
            <button class="delete-button" @click="removeTodo(todo)">Delete</button>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<script>
import { ref, onMounted, computed, watch } from 'vue';

export default {
  name: 'App',
  setup() {
    // define  variables
    const todos = ref([]);
    const name = ref('');
    const input_content = ref('');
    

    // computed property for sorted todos
    const sortedTodos = computed(() =>
      todos.value.slice().sort((a, b) => b.createdAt - a.createdAt)
    );

    // Function to add a new to-do item
    
    const addTodo = () => {
      if (input_content.value.trim() === '' ) {
        return;
      }
      todos.value.push({
        content: input_content.value,
        done: false,
        createdAt: new Date().getTime(),
      });

      // Clear input fields after adding todo
      input_content.value = '';
    
    };

    // Function to remove a to-do item
    const removeTodo = (todo) => {
      todos.value = todos.value.filter((t) => t !== todo);
    };

    // Watch for changes in todos and update local storage
    watch(todos, (newVal) => {
      localStorage.setItem('todos', JSON.stringify(newVal));
    }, { deep: true });

    // Watch for changes in name and update local storage
    watch(name, (newVal) => {
      localStorage.setItem('name', newVal);
    });

    // Load data from local storage on component mount
    onMounted(() => {
      name.value = localStorage.getItem('name') || '';
      todos.value = JSON.parse(localStorage.getItem('todos')) || [];
    });

    // Return variables and functions for the component
    return {
      name,
      input_content,
      sortedTodos,
      addTodo,
      removeTodo,
    };
  },
};
</script>

<!-- <style>
.app {
  background-color: #efeaea;
  min-height: 100vh;
  padding: 2rem;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.greeting {
  background-color: rgb(244, 100, 124);
  padding: 1.5rem;
  margin-bottom: 2rem;
  border-radius: 0.75rem;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 75%;
}

.title {
  font-size: 1.5rem;
  margin-bottom: 1rem;
}

.create-todo {
  background-color: rgb(244, 100, 124);
  padding: 1.5rem;
  margin-bottom: 2rem;
  border-radius: 0.75rem;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 75%;
}

.create-todo-heading {
  font-size: 1.25rem;
  margin-bottom: 1rem;
}

.mb-6 {
  margin-bottom: 1.5rem;
}

.block {
  display: block;
}

.text-lg {
  font-size: 1.125rem;
}

.font-semibold {
  font-weight: 600;
}

.border-b-2 {
  border-bottom-width: 2px;
}

.border-red-800 {
  border-color: #8b0000;
}

.focus\:outline-none:focus {
  outline: none;
}

.placeholder-black::placeholder {
  color: #000;
}

.bg-blue-700 {
  background-color: #0077cc;
}

.text-white {
  color: #fff;
}

.py-2 {
  padding-top: 0.5rem;
  padding-bottom: 0.5rem;
}

.py-3 {
  padding-top: 0.75rem;
  padding-bottom: 0.75rem;
}

.px-6 {
  padding-left: 1.5rem;
  padding-right: 1.5rem;
}

.md\:py-3 {
  padding-top: 0.75rem;
  padding-bottom: 0.75rem;
}

.md\:px-8 {
  padding-left: 2rem;
  padding-right: 2rem;
}

.rounded {
  border-radius: 0.25rem;
}

.hover\:bg-blue-800:hover {
  background-color: #005aa3;
}

.todo-list {
  background-color: rgb(244, 100, 124);
  padding: 1.5rem;
  border-radius: 0.75rem;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 75%;
}

.todo-item {
  display: flex;
  align-items: center;
  margin-bottom: 1rem;
}

.done {
  text-decoration: line-through;
}

.text-red-800 {
  color: #8b0000;
}

.inline-block {
  display: inline-block;
}

.w-4 {
  width: 1rem;
}

.h-4 {
  height: 1rem;
}

.rounded-full {
  border-radius: 9999px;
}

.mr-4 {
  margin-right: 1rem;
}

.flex-grow {
  flex-grow: 1;
}

.actions {
  margin-right: 1rem;
}

.delete {
  border-width: 1px;
  border-style: solid;
  color: #000;
  background-color: #fff;
  border-radius: 0.25rem;
  padding: 0.25rem 0.5rem;
  cursor: pointer;
}

.delete:hover {
  background-color: #000;
  color: #fff;
}

</style> -->
<style>
  /* Resetting default margin and padding */
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  /* Global styles */
  body {
    font-family: Arial, sans-serif;
    background-color: #6fbbfd;
  }

  .app {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
  }

  /* Greeting section */


  .greeting,
  .create-todo,
  .todo-list {
    background-color: #4CAF50; /* Green background color */
    color: rgb(10, 9, 9); /* White text color */
    padding: 20px; /* Padding around the content */
    border-radius: 10px; /* Rounded corners */
    margin-bottom: 20px; /* Spacing between sections */
  }
   .greeting {
    background-color: #4CAF50;
    color: rgb(16, 13, 13);
    padding: 20px;
    border-radius: 10px;
    margin-bottom: 20px;
  } 
  .greeting input[type="text"] {
    padding: 10px;
    border: 2px solid #4CAF50; /* Green border */
    border-radius: 5px;
    background-color: #4CAF50; /* Light gray background */
    transition: border-color 0.3s ease, background-color 0.3s ease;
    width: 250px; /* Adjust width as needed */
     /* Making the input text bold */
     font-size: 40px;

  }

  .greeting input[type="text"]:focus {
    border-color: #45a049; /* Darker green border on focus */
    background-color: #45a049; /* White background on focus */
  }


  /* Placeholder style */
  .greeting input[type="text"]::placeholder {
    color: #060606; /* Light gray placeholder text */
    font-size: 40px;

  }

  .title {
    font-size: 40px;
    font-weight: bold;
  }

  .create-todo-heading,
  .todo-list-heading {
    font-size: 20px;
    margin-bottom: 10px;
  }

  /* Form and input styles */
  .create-todo form {
    display: flex;
    align-items: center;
   
  }

  .input-container {
    margin-right: 10px;
  }

  .input-container label {
    font-weight: bold;
    color: #060606;
  }

  .input-container input[type="text"] {
    width: 300px;
    padding: 10px;
    border-radius: 5px;
    border: 1px solid #ccc;
    transition: border-color 0.3s ease;
      .greeting,
  .create-todo,
  .todo-list {
    background-color: #4CAF50; /* Green background color */
    color: white; /* White text color */
    padding: 20px; /* Padding around the content */
    border-radius: 10px; /* Rounded corners */
    margin-bottom: 20px; /* Spacing between sections */
  }
  }

  .input-container input[type="text"]:focus {
    border-color: #4CAF50;
  }

  .add-todo-button {
    background-color: #131c99;
    color: white;
    border: none;
    padding: 10px 20px;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }

  .add-todo-button:hover {
    background-color: #45a049;
  }

  /* Todo list styles */
  .list {
    margin-top: 20px;
  }

  .todo-item {
    background-color: #45a049;
    padding: 10px;
    border-radius: 5px;
    margin-bottom: 10px;
    display: flex;
    align-items: center;
  }

  .todo-item.done {
    opacity: 0.6;
  }

  .checkbox {
    margin-right: 10px;
  }

  .bubble {
    width: 15px;
    height: 15px;
    border-radius: 50%;
    display: inline-block;
    margin-right: 10px;
  }

  /* Colors for bubbles */
  .bubble.work {
    background-color: #4CAF50;
  }

  .bubble.personal {
    background-color: #2196F3;
  }

  .bubble.shopping {
    background-color: #FFC107;
  }

  /* Actions styles */
  .actions {
    margin-left: auto;
  }

  .delete-button {
    background-color: #f44336;
    color: white;
    border: none;
    padding: 5px 10px;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }

  .delete-button:hover {
    background-color: #d32f2f;
  }
  
</style>
