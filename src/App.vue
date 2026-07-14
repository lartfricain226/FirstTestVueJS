<!-- My todolist -->
<template>

  <Layout>
    <template v-slot:header>
      <h1>Accueil</h1>
    </template>
    <template v-slot:aside>
      <h1>Mes Posts</h1>
      <ol>
        <li v-for="post in posts" :key="post.id">
          {{ post.title }}
          <!-- <router-link :to="{ name: 'post', params: { id: post.id } }">
            {{ post.title }}
          </router-link> -->
        </li>
      </ol>
    </template>
    <template v-slot:main>
      <p>
      <form action="" @submit.prevent="addTask">
        <fieldset role="group">

          <input type="text" placeholder="Ajouter une tache" v-model="taskTitle">
          <button :disabled="taskTitle.length === 0">Ajouter</button>
        </fieldset>
      </form>


      <div v-if="tasks.length === 0"> Vous n'avez aucune tâche pour le moment</div>
      <div v-else>
        <table border="1">
          <tr>
            <th>Titre</th>
            <th>Statut</th>
            <th>Date de création</th>
            <th>Actions</th>
          </tr>
          <tr v-for="task in shortTasks" :key="task.date" :class="{ completed: task.status }">
            <td>{{ task.title }}</td>
            <td v-if="task.status === false">En cours</td>
            <td v-else>Effectuée</td>
            <td>{{ task.date }}</td>
            <td>
              <label for="">
                <input type="checkbox" v-model="task.status">
                Effectuée
              </label>
              <button @click="deleteTask(task)">x</button>
            </td>

          </tr>
        </table>

        <label>
          <input type="checkbox" v-model="hideTasksCompleted" :disabled="showTasksCompleted">
          Afficher les tâches en cours
        </label>
        <label>
          <input type="checkbox" v-model="showTasksCompleted" :disabled="hideTasksCompleted">
          Afficher les tâches effectuées
        </label>
        <p v-if="remainingTasks > 0">Il vous reste {{ remainingTasks }} tâche{{ remainingTasks > 1 ? "s" : "" }} à faire
        </p>
      </div>
      </p>
    </template>
    <template v-slot:footer>
      <p >@Copyright 2026 - All rights reserved</p>
    </template>
  </Layout>

  <hr>

  <FirstVue title="La page FirstVue" @movie-deleted="handleMovieDeleted" @milestone-reached="handleMilestone" />
  <p v-if="notification">{{ notification }}</p>

  <hr>




</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import FirstVue from './FirstVue.vue'
import Layout from './Layout.vue'
const posts = ref([])
onMounted(() => {
  fetch('https://jsonplaceholder.typicode.com/posts?_limit=10')
  .then(res => res.json())
  .then(json => posts.value = json)
})

const tasks = ref([
  {
    title: "Faire les courses",
    status: true,
    date: new Date().toLocaleDateString()
  },
  {
    title: "Faire la vaisselle",
    status: false,
    date: new Date().toLocaleDateString()
  }
])

const taskTitle = ref("")
const taskDate = ref(new Date().toLocaleDateString())

const addTask = () => {
  if (taskTitle.value.trim() === "") {
    return
  }
  tasks.value.push({
    title: taskTitle.value,
    status: false,
    date: taskDate.value
  })
  taskTitle.value = ""
}

const deleteTask = (task) => {
  tasks.value = tasks.value.filter(t => t.title !== task.title)
}

const shortTasks = computed(() => {
  const shortedTasks = tasks.value.toSorted((a, b) => a.status > b.status ? 1 : -1)

  if (hideTasksCompleted.value) {
    return shortedTasks.filter(t => !t.status)
  } else if (showTasksCompleted.value) {
    return shortedTasks.filter(t => t.status)
  } else {
    return shortedTasks
  }
})

const hideTasksCompleted = ref(false)
const showTasksCompleted = ref(false)
const remainingTasks = computed(() => tasks.value.filter(t => !t.status).length)

const notification = ref("")

const handleMovieDeleted = (movieName) => {
  notification.value = `Film "${movieName}" supprimé avec succès !`
  setTimeout(() => notification.value = "", 3000)
}

const handleMilestone = (count) => {
  notification.value = `🎉 Vous avez atteint ${count} clics dans FirstVue !`
  setTimeout(() => notification.value = "", 3000)
}
</script>

<style scoped>
.completed {
  opacity: .5;
}

.completed td {
  text-decoration: line-through;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th {
  text-align: left;
  background: var(--pico-secondary-background);
}

tr {
  transition: opacity 0.2s ease;
}

.completed {
  opacity: 0.5;
}

.completed td {
  text-decoration: line-through;
}

/* Badge de statut au lieu de texte brut */
td:nth-child(2) {
  font-weight: 600;
}

fieldset[role="group"] {
  margin-bottom: 1.5rem;
}

label {
  display: block;
  margin-top: 0.5rem;
}
</style>
