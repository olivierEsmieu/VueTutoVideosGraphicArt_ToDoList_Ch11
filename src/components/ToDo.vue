<template>
  <div class="ui main container">
    <div class="ui grid">
      <div class="eight wide column">
        <input type="text" placeholder="nouvelle tâche" v-model="inputTodoName" @keyup.enter="add">
      </div>
      <div class="eight wide column">
        <button class="ui toggle button" @click.prevent="checkUnchekAll">{{cocheDecocheTout}}</button>
      </div>
    </div>
    <div class="ui divided middle aligned divided list">
      <div
        v-for="(todo,i) in todos"
        :key="i"
        v-show="filter === 'todo' ? todo.completed === false: filter === 'done'? todo.completed === true:true"
      >
        <input type="checkbox" :id="i" v-model="todo.completed">
        <label @dblclick="editTodo(todo)" v-show="editing === null">{{todo.name}}</label>
        <input
          type="text"
          v-show="editing === todo"
          v-focus="editing === todo"
          @keyup.escape="cancelEdit"
          @keyup.enter="doneEdit"
          @blur="doneEdit"
          v-model="todo.name"
        >
        <button @click.prevent="deleteTodo(i)">supprime</button>
      </div>
    </div>
    <footer v-show="todos.length > 0">
      <span>
        <strong>{{ todosUncompletedCount }}</strong> tâche(s) restante(s)
      </span>
      <div class="ui three item menu">
        <a class="item" :class="{ active: filter == 'all' }" @click.prevent="filter = 'all';">toutes</a>
        <a
          class="item"
          :class="{ active: filter == 'todo' }"
          @click.prevent="filter = 'todo';"
        >à faire</a>
        <a
          class="item"
          :class="{ active: filter == 'done' }"
          @click.prevent="filter = 'done';"
        >faites</a>
      </div>
    </footer>
    <pre>
    todos:    {{todos}}
    </pre>
  </div>
</template>

<script>
import Vue from "vue";
export default {
  name: "ToDo",
  props: {},
  data: function() {
    return {
      todos: [{ name: "tache de test", completed: false }],
      inputTodoName: "",
      filter: "all",
      editing: null,
      cocheDecocheTout: "coche tout",
      oldTodo: ""
    };
  },
  directives: {
    focus(el, value) {
      if (value) {
        Vue.nextTick(_ => {
          el.focus();
        });
      }
    }
  },
  computed: {
    todosUncompletedCount() {
      return this.todos.filter(todo => todo.completed === false).length;
    }
  },
  methods: {
    editTodo(todo) {
      this.editing = todo;
      this.oldTodo = todo.name;
    },
    doneEdit() {
      this.editing = null;
    },
    cancelEdit() {
      this.editing.name = this.oldTodo;
      this.doneEdit();
    },
    add() {
      this.todos.push({
        name: this.inputTodoName,
        completed: false
      });
      this.inputTodoName = "";
    },
    checkUnchekAll() {
      if (this.cocheDecocheTout === "coche tout") {
        this.todos.map(todo => (todo.completed = true));
        this.cocheDecocheTout = "décoche tout";
      } else {
        this.todos.map(todo => (todo.completed = false));
        this.cocheDecocheTout = "coche tout";
      }
    },
    deleteTodo(i) {
      this.todos.splice(i, 1);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
