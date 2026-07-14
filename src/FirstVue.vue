<template>
  <Layout>
    <template v-slot:header>
      <h1>Ma première page</h1>
    </template>
    <template v-slot:aside>
      <p>Barre latérale</p>
    </template>
      <p>Barre latérale</p>
    <template v-slot:main>
      <h1>Hello {{ person.firstName.toUpperCase() }}, vous êtes sur  {{ title }}</h1>
  <table border="1">
    <tr>
      <th>Nom</th>
      <th>Prénom</th>
      <th>Age</th>
      <th v-if="person.sex">Sexe</th>
    </tr>
    <tr>
      <td>{{ person.firstName }}</td>
      <td>{{ person.lastName }}</td>
      <td>
        {{ person.age }}
      </td>
      <td v-if="person.sex">{{ person.sex }}</td>
    </tr>
  </table>
  
  <p>Votre compteur: {{ count }}</p>

  <h2 :class="{ active: count2 > 0 }">
    Incrémenter/décrémenter
  </h2>
  <div class="buttons">
    <button @mousedown="startIncrement" @mouseup="leaveMouse">+1
    </button>
    <button @click="count2--">-1
    </button>
    
    <p v-if="isPressed">Vous êtes en train d'incrémenter</p>
  </div>
  <p :id="`total-${count2}`"
    :style="{ fontSize: count2 >= 15 ? count2 + 'px' : '14px', color: `${count2 >= 20 ? 'green' : 'red'}` }">
    Nombre de clicks: {{ count2 }}
  </p>
  <p v-if="count2 >= 20" style="font-style: italic;font-size: 20px">Bravo, vous avez fait plus de 20 clicks</p>
  <p v-else>Vous avez fait moins de 20 clicks</p>


  <h2>Movies</h2>
  <hr>
  <form action="" @submit.prevent="addMovie">
    <input type="text" v-model="movieName" placeholder="Nom du film">
    <input type="date" v-model="movieYear" placeholder="Anné de sortie">
    
    <button >Ajouter un film</button>
  </form>
  <hr>
  <ul>
    <li v-for="movie in movies" :key="movie.name">
      {{ movie.name }} plublié en {{ movie.year }} 
      <button @click="deleteMovie(movie)">Supprimer</button>
    </li>
    
  </ul>
  <hr>
  <button @click="shortByNameAZ">
    Trier par ordre alphabetique
  </button>
    </template>
    <template v-slot:footer>
      <p>Pied de page</p>
    </template>
  </Layout>
</template>

<script setup>
import { ref, onUnmounted } from 'vue'
import Layout from './Layout.vue'

const props = defineProps({
  title: String
})

const emits = defineEmits([
'movie-deleted', 'milestone-reached'
])

const count = ref(0)
const interval = setInterval(() => {
  count.value++
}, 1000)

onUnmounted(() => {
  clearInterval(interval)
})

const count2 = ref(0)
const isPressed = ref(false)
const startIncrement = () => {
  count2.value++
  isPressed.value = true
  if (count2.value === 20) {
    emits("milestone-reached", count2.value)
  }
}

const leaveMouse = () => {
  isPressed.value = false
}


const movies = ref([
  { name: "The Shawshank Redemption", year: 1994 },
  { name: "The Godfather", year: 1972 },
  { name: "The Dark Knight", year: 2008 }
])


const deleteMovie = (movie) => {
  movies.value = movies.value.filter(m => m.name !== movie.name)
  emits("movie-deleted", movie.name)
}

const shortByNameAZ = () => {
  // movies.value.sort((a, b) => a.name.localeCompare(b.name))
  movies.value.sort((a, b) => a.name>b.name ? 1 : -1)
}

const movieName = ref("")
const movieYear = ref()
const addMovie = () => {
  if (movieName.value.trim() === "" || !movieYear.value) {
    return
  }
  movies.value.push({
    name: movieName.value,
    year: new Date(movieYear.value).getFullYear()
  })
  movieName.value = ""
  movieYear.value = ""
}

const person = ref({
  firstName: "SIMBRE",
  lastName: "Alassane",
  age: 25
})


</script>

<style scoped>
h1 {
  color: red
}

.active {
  color: green
}

.buttons {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

h1 {
  color: var(--pico-primary);
  margin-bottom: 1.5rem;
}

.active {
  color: var(--pico-ins-color, green);
  font-weight: bold;
}

.buttons {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 20px;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 1.5rem;
}

th {
  background: var(--pico-secondary-background);
  text-align: left;
}

form {
  display: flex;
  gap: 10px;
  margin-bottom: 1.5rem;
  flex-wrap: wrap;
}

form input {
  flex: 1;
  min-width: 150px;
}
</style>
