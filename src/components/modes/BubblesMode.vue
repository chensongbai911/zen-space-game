<script setup lang="ts">
import { ref, onMounted } from 'vue'

const bubbles = ref<any[]>([])

const createBubble = (id: number) => ({
  id,
  position: [
    (Math.random() - 0.5) * 10,
    (Math.random() - 0.5) * 6 + 2,
    (Math.random() - 0.5) * 10
  ],
  scale: Math.random() * 0.5 + 0.5,
  color: `hsl(${Math.random() * 360}, 70%, 70%)`,
})

onMounted(() => {
  for (let i = 0; i < 30; i++) {
    bubbles.value.push(createBubble(i))
  }
})
</script>

<template>
  <TresGroup>
    <TresMesh
      v-for="bubble in bubbles"
      :key="bubble.id"
      :position="bubble.position"
      :scale="[bubble.scale, bubble.scale, bubble.scale]"
    >
      <TresSphereGeometry :args="[0.5, 32, 32]" />
      <TresMeshPhysicalMaterial
        :color="bubble.color"
        :transmission="0.8"
        :thickness="0.5"
        :roughness="0.1"
        :metalness="0.1"
        :clearcoat="1"
      />
    </TresMesh>
  </TresGroup>
</template>
