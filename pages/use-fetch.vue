<template>
  <h1>Use Fetch</h1>

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

const endpoint = computed(() => {
  return `https://pokeapi.co/api/v2/pokemon/${selectedPokemon.value}`
})

// usefetch redirects only when request is resolved
//useLazyFetch redirects to page without resolving the request
  const { data: charmander, pending, error, refresh  } = await useFetch(endpoint, {
    method: "GET",
    headers: {
      "Content-Type": "application/json"
    },
    transform: (data) => ( {
      id: data.id,
      name: data.name,
      image: data.sprites.front_default
    }),
  })
</script>