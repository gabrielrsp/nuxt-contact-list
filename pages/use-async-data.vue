<template>
  <h1>Use Async Data</h1>

  <button @click="refresh"> Reload Data</button>

  <div v-if="pending">Loading...</div>

  <div v-else-if="error">Error: {{ error.message }}</div>

  <pre v-else>
    {{ charmander }}
  </pre>

  <div>
    <select v-model="selectedPokemon" name="pokemon">
      <option value="charmander">Charmander</option>
      <option value="bulbasaur">Bulbasaur</option>
      <option value="pikachu">Pikachu</option>
      <option value=""></option>
    </select>
  </div>

</template>

<script setup>

const selectedPokemon = ref("charmander");

// const endpoint = computed(() => {
//   return `https://pokeapi.co/api/v2/pokemon/${selectedPokemon.value}`
// })

// usefetch redirects only when request is resolved
//useLazyFetch redirects to page without resolving the request

  const { data: charmander, pending, error, refresh  } = await useAsyncData("pokemon-info",
   async () => {
      const [pokemonData, speciesData] = await Promise.all([
        $fetch(`https://pokeapi.co/api/v2/pokemon/${selectedPokemon.value}`),
        $fetch(`https://pokeapi.co/api/v2/pokemon-species/${selectedPokemon.value}`),
      ])

      return {
        id: pokemonData.id,
        name: pokemonData.name,
        image: pokemonData.sprites.front_default,
        habitat: speciesData.habitat?.name,
        shape: speciesData.shape?.name
      }
    },
    {
      watch: [selectedPokemon],
      lazy: true
    }  )
</script>