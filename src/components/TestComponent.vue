<template>
  <div class="todo-app">
    <h1>Todo App</h1>
    <div>
      <input v-model="newTodoText" placeholder="Enter a new todo" />
      <button @click="addTodo">Add Todo</button>
    </div>
    <br />
    <div>
      <input v-model="newFolderName" placeholder="Enter folder name" />
      <button @click="addFolder">Add Folder</button>
    </div>
    <br />
    <div>
      <select v-model="selectedFolder">
        <option disabled value="">Select a folder</option>
        <option v-for="(folder, folderIndex) in folders" :key="folderIndex" :value="folder">
          {{ folder.name }}
        </option>
      </select>
    </div>
    <br />
    <div v-if="selectedFolder">
      <h3>{{ selectedFolder.name }}</h3>
      <ul>
        <li v-for="(todo, todoIndex) in selectedFolder.todos" :key="todoIndex">
          <span :class="{ 'strike-through': todo.completed }">
            {{ todo.text }}
          </span>
          <button @click="toggleCompleted(todo)">Done</button>
          <button @click="editTodoFolder(todo)">Edit</button>
          <button @click="deleteTodoFolder(todo)">Delete</button>
        </li>
      </ul>
    </div>
    <div v-else>
      <p>No folder selected</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      folders: [
        {
          name: 'Default',
          todos: [],
        },
        {
          name: 'Work',
          todos: [],
        },
        {
          name: 'Personal',
          todos: [],
        },
      ],
      newTodoText: '',
      newFolderName: '',
      selectedFolder: null,
    };
  },
  methods: {
    addTodo() {
      if (this.selectedFolder) {
        this.selectedFolder.todos.push({
          text: this.newTodoText,
          completed: false,
        });
        this.newTodoText = '';
      }
    },
    toggleCompleted(todo) {
      todo.completed = !todo.completed;
    },
    editTodoFolder(todo) {
      const newTodoText = prompt('Enter the new todo text:', todo.text);
      if (newTodoText) {
        todo.text = newTodoText;
      }
    },
    deleteTodoFolder(todo) {
      const index = this.selectedFolder.todos.indexOf(todo);
      if (index !== -1) {
        this.selectedFolder.todos.splice(index, 1);
      }
    },
    addFolder() {
      const newFolder = {
        name: this.newFolderName,
        todos: [],
      };
      this.folders.push(newFolder);
      this.newFolderName = '';
    },
  },
};
</script>

<style>
.strike-through {
  text-decoration: line-through;
}
.todo-app {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f7f7f7;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.todo-app h2 {
  margin-top: 50;
  margin-bottom: 20px;
  font-size: 24px;
  text-align: center;
  }
</style>
