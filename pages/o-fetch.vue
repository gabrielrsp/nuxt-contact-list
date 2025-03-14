<template>
  <h1>OFetch</h1>

  <div v-if="pending">Loading... </div>
  <div v-else-if="error">Error: {{ error.message }}</div>
  <pre v-else>
    {{ userProfile }}
  </pre>

  <button @click="fetchUserProducts">Load Products</button> 

 <div v-if="isLoadingProducts">Loading Products</div>

  <div v-else-if="userProducts.length > 0">
    <ul>
      <li v-for="product in userProducts" :key="product.id">
        {{ product.name }} - {{ product.price }}
      </li>
    </ul>
  </div>
</template>

<script  setup lang="ts">
import type { IUserProduct } from '~/services/user/types'

  const { $http } = useNuxtApp()

  const {data: userProfile, pending, error} = await useAsyncData('user-profile', () => $http.user.getUserProfile())

  const isLoadingProducts = ref(false)
  const userProducts = ref<IUserProduct[]>([])

 
  // UseFetch e UseAsyncData apenas quando quero carregar uma informação na inicialização da página
  // Não devo usar esses composables quando uso GET e nao preciso das infos logo que a tela é carregada
  // Se for uma requisição do tipo POST PUT ou DELETE eu não uso esses composables

  const fetchUserProducts = async () => {
    try {
      isLoadingProducts.value = true
      userProducts.value = await $http.user.getUserProducts();
    } catch (error: any) {
      alert(error.message)
    } finally {
      isLoadingProducts.value = false
    }
  }

</script> 