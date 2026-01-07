<script setup lang="ts">
const count = 2000
const positions = new Float32Array(count * 3)
const colors = new Float32Array(count * 3)

for (let i = 0; i < count; i++) {
  positions[i * 3] = (Math.random() - 0.5) * 15
  positions[i * 3 + 1] = (Math.random() - 0.5) * 15
  positions[i * 3 + 2] = (Math.random() - 0.5) * 15
  
  colors[i * 3] = 0.5 + Math.random() * 0.5
  colors[i * 3 + 1] = 0.2 + Math.random() * 0.3
  colors[i * 3 + 2] = 0.8 + Math.random() * 0.2
}
</script>

<template>
  <TresPoints>
    <TresBufferGeometry>
      <TresBufferAttribute
        attach="attributes-position"
        :count="count"
        :array="positions"
        :item-size="3"
      />
      <TresBufferAttribute
        attach="attributes-color"
        :count="count"
        :array="colors"
        :item-size="3"
      />
    </TresBufferGeometry>
    <TresPointsMaterial
      :size="0.1"
      :vertex-colors="true"
      :transparent="true"
      :opacity="0.8"
      :size-attenuation="true"
    />
  </TresPoints>
</template>
