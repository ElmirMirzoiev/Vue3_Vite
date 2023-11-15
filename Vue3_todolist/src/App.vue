<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('')

const input_content = ref('')
const input_category = ref(null)

const todos_asc = computed(() => todos.value.sort((a, b) => {
  return b.createdAt - a.createdAt
}))

const addTodo = () => {
  if (input_content.value.trim() === '' || input_content.value === null) {
    return
  }

  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    createdAt: new Date.getTime(),
    done: false
  })
}

watch(todos, newVal => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, { deep: true })

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
})



</script>

<template>
  <main class="app">

    <section class="greeting">
      <h2 class="title">
        What's up, <input type="text" placeholder="Name here" v-model="name" />
      </h2>
    </section>

    <section class="create-todo">
      <h3>Create Todo</h3>

      <form @submit.prevent="addTodo">
        <h4>What do you need to do></h4>
        <input type="text" placeholder="e.g. make a video" v-model="input_content">

        <h4>Pick a category</h4>

        <div class="options">

          <label>
            <input type="radio" name="category" id="category1" value="business" v-model="input_category">
            <span class="bubble bussiness"></span>
            <div>Business</div>
          </label>

          <label>
            <input type="radio" name="category" id="category2" value="Personal" v-model="input_category">
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>

        </div>

        <input type="submit" value="Add todo">

      </form>
    </section>

  </main>
</template>


