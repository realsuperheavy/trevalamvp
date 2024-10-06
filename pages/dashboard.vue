<template>
  <div>
    <h1 class="text-3xl font-bold mb-4">Dashboard</h1>
    <nav class="mb-4">
      <NuxtLink to="/bucket-list" class="btn btn-primary mr-2">Bucket List</NuxtLink>
      <NuxtLink to="/trips" class="btn btn-primary mr-2">Trips</NuxtLink>
      <NuxtLink to="/progress" class="btn btn-primary mr-2">Progress</NuxtLink>
      <NuxtLink to="/recommendations" class="btn btn-primary">Recommendations</NuxtLink>
    </nav>
    <MapView />
  </div>
</template>

<script setup>
import MapView from '~/components/MapView.vue'
</script>