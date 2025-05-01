<script setup>

import {ref, watch} from "vue";
import Draggable from 'vuedraggable'

const newTodo = ref('')
const todos = ref([])
const completedTodos = ref([])

function addTodo() {
  if (newTodo.value.trim()) {
    todos.value.push({text: newTodo.value, completed: false})
    newTodo.value = ''
  }
}

function removeTodo(index) {
  todos.value.splice(index, 1)
}

function moveItem(moveTodo) {
  todos.value = todos.value.filter(todo => todo !== moveTodo)
  completedTodos.value = completedTodos.value.filter(todo => todo !== moveTodo)
  if (moveTodo.completed) {
    completedTodos.value.push(moveTodo)
  } else {
    todos.value.push(moveTodo)
  }
}

</script>

<template>

    <div>
      <input v-model="newTodo" placeholder="Add New Todo item">
      <button @click="addTodo">Add</button>
    </div>

    <div>
      <Draggable v-model="todos">
        <template #item="{ element, index }">
          <div>
            <input type="checkbox" v-model="element.completed" @change="moveItem(element)"/>
            <span :style="{ textDecoration: element.completed ? 'line-through' : 'none' }" class="draggable-item">{{ element.text }}</span>
            <a href="#" @click.prevent="removeTodo(index)">Remove</a>
          </div>
        </template>
      </Draggable>
    </div>

    <div>
      <div>
        <h3>Completed</h3>
        <Draggable v-model="completedTodos">
          <template #item="{ element, index }">
            <div>
              <input type="checkbox" v-model="element.completed" @change="moveItem(element)"/>
              <span :style="{ textDecoration: element.completed ? 'line-through' : 'none' }" class="draggable-item">{{ element.text }}</span>
              <a href="#" @click.prevent="removeTodo(index)">Remove</a>
            </div>
          </template>
        </Draggable>
      </div>
    </div>

    <TheWelcome />

</template>

<style scoped>
.draggable-item {
  cursor: grab;
}

.draggable-item:active {
  cursor: grabbing;
}
</style>
