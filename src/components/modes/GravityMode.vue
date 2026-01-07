<script setup lang="ts">
import { ref, onMounted } from 'vue'

const boxes = ref<any[]>([])

const createBox = () => ({
  id: Math.random(),
  position: [
    (Math.random() - 0.5) * 8,
    5 + Math.random() * 3,
    (Math.random() - 0.5) * 8
  ],
  rotation: [Math.random() * Math.PI, Math.random() * Math.PI, Math.random() * Math.PI],
  color: `hsl(${200 + Math.random() * 40}, 80%, 60%)`,
  size: Math.random() * 0.5 + 0.5
})

onMounted(() => {
  for (let i = 0; i < 15; i++) {
    boxes.value.push(createBox())
  }
})
</script>

<template>
  <TresGroup>
    <!-- Ground -->
    <TresMesh :rotation="[-Math.PI / 2, 0, 0]" :position="[0, 0, 0]">
      <TresPlaneGeometry :args="[20, 20]" />
      <TresMeshStandardMaterial color="#1e293b" />
    </TresMesh>

    <!-- Boxes -->
    <TresMesh
      v-for="box in boxes"
      :key="box.id"
      :position="box.position"
      :rotation="box.rotation"
    >
      <TresBoxGeometry :args="[box.size, box.size, box.size]" />
      <TresMeshStandardMaterial :color="box.color" :roughness="0.3" :metalness="0.2" />
    </TresMesh>
  </TresGroup>
</template>
