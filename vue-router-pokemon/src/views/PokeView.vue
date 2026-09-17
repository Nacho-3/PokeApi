<script setup>
import axios from 'axios'
import { ref, onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'

const route = useRoute()
const router = useRouter()

const pokemon = ref(null)
const loading = ref(true)
const error = ref(null)

const getPokemon = async () => {
  try {
    const response = await axios.get(
      `https://pokeapi.co/api/v2/pokemon/${route.params.name}`
    )

    pokemon.value = response.data
  } catch (requestError) {
    error.value = 'Pokémon no encontrado.'
  } finally {
    loading.value = false
  }
}

onMounted(getPokemon)

const goBack = () => {
  router.push('/pokemons')
}
</script>

<template>
  <main>
    <p v-if="loading">Cargando...</p>
    <p v-else-if="error">{{ error }}</p>

    <section v-else>
      <img
        :src="pokemon.sprites.front_default"
        :alt="pokemon.name"
      />

      <h1>{{ pokemon.name }}</h1>
      <p>Altura: {{ pokemon.height }}</p>
      <p>Peso: {{ pokemon.weight }}</p>

      <button @click="goBack">
        Volver al listado
      </button>
    </section>
  </main>
</template>