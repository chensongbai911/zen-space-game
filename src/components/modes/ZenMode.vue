<script setup lang="ts">
</script>

<template>
  <TresGroup>
    <TresMesh :position="[0, 2, 0]">
      <TresTorusKnotGeometry :args="[1.5, 0.5, 128, 32]" />
      <TresMeshPhysicalMaterial
        color="#10b981"
        :roughness="0.1"
        :metalness="0.8"
        :transmission="0.5"
        :thickness="1"
        :ior="1.5"
      />
    </TresMesh>

    <!-- Decorative elements -->
    <TresMesh v-for="i in 5" :key="i" :position="[Math.sin(i) * 5, 0.5, Math.cos(i) * 5]">
      <TresSphereGeometry :args="[0.3, 32, 32]" />
      <TresMeshStandardMaterial color="#ffffff" :emissive="'#10b981'" :emissive-intensity="0.5" />
    </TresMesh>

    <TresGridHelper :args="[20, 20, '#10b981', '#064e3b']" :position="[0, 0, 0]" />
  </TresGroup>
</template>
