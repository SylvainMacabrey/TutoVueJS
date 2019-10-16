<template>
  <section class="todoapp">
      <header class="header">
          <h1>Todos</h1>
          <input type="text" class="new-todo" placeholder="Ajouter une tâche" v-model="newTodo" @keyup.enter="addTodo">
      </header>
      <div class="main">
          <input type="checkbox" class="toggle-all" v-model="allDone">
          <ul class="todo-list">
              <li class="todo" :class="{completed: todo.completed, editing: todo === editing}" v-for="todo in filteredTodos">
                  <div class="view">
                      <input type="checkbox" class="toggle" v-model="todo.completed">
                      <label @dblclick="editTodo(todo)">{{ todo.name }}</label>
                      <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
                  </div>
                  <input type="text" class="edit" v-model="todo.name" @blur="doneEdit" @keyup.enter="doneEdit" @keyup.esc="cancelEdit" v-focus="todo === editing">
              </li>
          </ul>
      </div>
      <footer class="footer" v-show="todos.length > 0">
          <span class="todo-count"><strong>{{ remaining }}</strong> tâches à faire</span>
          <ul class="filters">
              <li><a href="#" :class="{selected: filter === 'all'}" @click.prevent="filter = 'all'">Toutes</a></li>
              <li><a href="#" :class="{selected: filter === 'todo'}" @click.prevent="filter = 'todo'">A faire</a></li>
              <li><a href="#" :class="{selected: filter === 'done'}" @click.prevent="filter = 'done'">Faite</a></li>
          </ul>
          <button class="clear-completed" @click.prevent="deleteCompleted" v-show="completed">Supprimer les tâches finis</button>
      </footer>
  </section>
</template>

<script>
import Vue from 'vue'
export default {
    data() {
        return {
            todos: [],
            newTodo: '',
            filter: 'all',
            editing: null,
            oldTodo: ''
        }
    },
    methods: {
        addTodo() {
            this.todos.push({
                name: this.newTodo,
                completed: false
            })
            this.newTodo = ''
        },
        deleteTodo(todo) {
            this.todos = this.todos.filter(t => t !== todo)
        },
        editTodo(todo) {
            this.editing = todo
            this.oldTodo = todo.name
        },
        doneEdit() {
            this.editing = null
        },
        cancelEdit() {
            this.editing.name = this.oldTodo
            this.doneEdit()
        },
        deleteCompleted() {
            this.todos = this.todos.filter(todo => !todo.completed)
        }
    },
    computed: {
        allDone: {
            get() {
                return this.remaining === 0
            },
            set(value) {
                this.todos.forEach(todo => {
                    todo.completed = value
                })
            }
        },
        remaining() {
            return this.todos.filter(todo => !todo.completed).length
        },
        completed() {
            return this.todos.filter(todo => todo.completed).length
        },
        filteredTodos() {
            if(this.filter === 'todo') {
                 return this.todos.filter(todo => !todo.completed)
            }
            if(this.filter === 'done') {
                 return this.todos.filter(todo => todo.completed)
            }
            return this.todos
        }
    },
    directives: {
        focus(el, value) {
            if(value) {
                Vue.nextTick(_ => {
                    el.focus()
                })
            }
        }
    }
}
</script>

<style src="./todos.css"></style>