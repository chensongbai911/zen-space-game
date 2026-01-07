<script setup lang="ts">
import { ref } from 'vue'
import { useRenderLoop } from '@tresjs/core'

const torusRef = ref()
const decorativeRef = ref<any[]>([])

const { onBeforeRender } = useRenderLoop()
onBeforeRender(({ elapsed }) => {
  if (torusRef.value) {
    torusRef.value.rotation.x = elapsed * 0.3
    torusRef.value.rotation.y = elapsed * 0.4
    torusRef.value.rotation.z = elapsed * 0.2
  }
  
  // 装饰元素的脉冲效果
  decorativeRef.value.forEach((mesh, i) => {
    if (mesh) {
      const scale = 0.3 + Math.sin(elapsed * 2 + i) * 0.1
      mesh.scale.set(scale, scale, scale)
    }
  })
})
</script>

<template>
  <TresGroup>
    <TresMesh ref="torusRef" :position="[0, 2, 0]">
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

    <!-- 装饰元素 - 脉冲效果 -->
    <TresMesh 
      v-for="i in 5" 
      :key="i"
      :ref="el => { if (el) decorativeRef[i-1] = el }"
      :position="[Math.sin(i) * 5, 0.5, Math.cos(i) * 5]"
    >
      <TresSphereGeometry :args="[0.3, 32, 32]" />
      <TresMeshStandardMaterial color="#ffffff" :emissive="'#10b981'" :emissive-intensity="0.8" />
    </TresMesh>

    <!-- 网格地面 -->
    <TresGridHelper :args="[20, 20, '#10b981', '#064e3b']" :position="[0, 0, 0]" />
  </TresGroup>
</template>
