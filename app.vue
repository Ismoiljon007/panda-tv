<template>
  <div class="overlay" v-if="store.overlay" @click="store.overlay = false, store.search_open = false"></div>
  <NuxtLayout />
  <loading />
</template>
<script setup lang="ts">
import { useStore } from './store/store';
import { useAuthStore } from "./store/auth"
const store = useStore()
const authStore = useAuthStore()
const route = useRoute()

watch(() => route.path, (newPath: any, oldPath: any) => {
  if (process.client) {
    const html = document.querySelector<any>('html')

    // Check both conditions before setting overflow
    if (store.loader || store.search_open) {
      html.style.overflow = 'hidden'
    } else {
      html.style.overflow = 'visible'
    }

    // Only focus when search_open is true
    if (store.search_open) {
      setTimeout(() => {
        document.getElementById('search-int')?.focus()
      }, 1000)
    }
  }
})
onMounted(() => {
  if (process.client) {
    const html = document.querySelector<any>('html')

    // Check both conditions before setting overflow
    if (store.loader || store.search_open) {
      html.style.overflow = 'hidden'
    } else {
      html.style.overflow = 'visible'
    }

    // Only focus when search_open is true
    if (store.search_open) {
      setTimeout(() => {
        document.getElementById('search-int')?.focus()
      }, 1000)
    }
  }
})


watchEffect(() => {
  if (process.client) {
    const item: string | null | undefined = localStorage.getItem("token");
    authStore.token = item
      ? JSON.parse(item)
      : null
    const html = document.querySelector<any>('html')
    // Check both conditions before setting overflow
    if (store.loader || store.search_open) {
      html.style.overflow = 'hidden'
    } else {
      html.style.overflow = 'visible'
    }

    // Only focus when search_open is true
    if (store.search_open) {
      setTimeout(() => {
        document.getElementById('search-int')?.focus()
      }, 1000)
    }
  }
})

</script>