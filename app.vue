<template>
  <div>
    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>
  </div>
</template>

<script setup>
import { useFirebase } from '~/composables/useFirebase'

const { initializeFirebase } = useFirebase()

// Initialize Firebase when the app starts
onMounted(() => {
  initializeFirebase()
})
</script>