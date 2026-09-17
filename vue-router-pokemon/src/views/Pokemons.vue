<script setup>
import axios from 'axios'
import { ref, onMounted } from 'vue'
import { RouterLink } from 'vue-router'

const pokemons = ref([])
const loading = ref(true)
const error = ref(null)

const getPokemons = async () => {
    try {
        const response = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=10')
        pokemons.value = response.data.results
    } catch (err) {
        error.value = err.message
    } finally {
        loading.value = false
    }
}

onMounted(() => {
    getPokemons()
})

</script>

<template>
  <main>
    <h1>Pokémon</h1>

    <p v-if="loading">Cargando...</p>
    <p v-else-if="error">{{ error }}</p>

    <ul v-else>
      <li v-for="pokemon in pokemons" :key="pokemon.name">
        <RouterLink :to="`/pokemons/${pokemon.name}`">
          {{ pokemon.name }}
        </RouterLink>
      </li>
    </ul>
  </main>
</template>