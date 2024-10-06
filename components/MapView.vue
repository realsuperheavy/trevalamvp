<template>
  <div style="height: 400px;">
    <ClientOnly>
      <l-map :zoom="13" :center="[51.505, -0.09]">
        <l-tile-layer url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"></l-tile-layer>
        <l-marker :lat-lng="[51.505, -0.09]"></l-marker>
      </l-map>
    </ClientOnly>
  </div>
</template>

<script setup>
import { LMap, LTileLayer, LMarker } from '@vue-leaflet/vue-leaflet'
</script>