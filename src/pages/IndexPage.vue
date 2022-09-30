<template>
  <q-page padding>
    <div class="row q-mb-lg">
      <q-input class="col" v-model="newTodo" label="Add Todo" :dense="dense" @keyup.enter="addTodo"/>
      <q-btn color="primary" label="Add" @click.native="addTodo" />
    </div>
    <div class="row">

  <div class="q-pa-md col" style="max-width: 350px">
    <q-list bordered separator>
      <q-list-header>My Todos</q-list-header>
      <q-item clickable v-ripple v-for="(todo, index) in todos" :key="todo.id">
        <q-item-section>{{todo}}</q-item-section>
        <q-item-section side>
          <q-icon name="check" color="green" @click.native="moveToDone(index)" />
        </q-item-section>
      </q-item>
    </q-list>
  </div>

    </div>

    <div class="row">

<div class="q-pa-md col" style="max-width: 350px">
  <q-list bordered separator>
    <q-list-header>Done Todos</q-list-header>
    <q-item clickable v-ripple v-for="(todo, index) in done" :key="todo.id">
      <q-item-section>{{todo}}</q-item-section>
      <q-item-section side>
        <q-icon name="close" color="red" @click.native="deleteTodo(index)" />
      </q-item-section>
    </q-item>
  </q-list>
</div>

  </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'IndexPage',
  data() {
    return {
      todos: [],
      newTodo: '',
      done: []
    }
  },
  methods: {
    addTodo() {
      this.todos.push(this.newTodo)
      this.newTodo = ''
    },
    moveToDone(index) {
      this.done.push(this.todos[index])
      this.todos.splice(index, 1)
    },
    deleteTodo(index) {
      this.$q.dialog({
        title: 'Confirm',
        message: 'Would you like to delete this Todo?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        
        if (this.done.splice(index, 1)) {
          this.$q.notify('Deleted!')
        }
        // console.log('>>>> OK')
      }).onOk(() => {
        // console.log('>>>> second OK catcher')
      }).onCancel(() => {
        // console.log('>>>> Cancel')
      }).onDismiss(() => {
        // console.log('I am triggered on both OK and Cancel')
      })
    }
  }
})
</script>
