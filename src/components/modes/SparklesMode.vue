<script setup lang="ts">
import { ref } from 'vue'
import { useRenderLoop } from '@tresjs/core'

const count = 3000
const positions = new Float32Array(count * 3)
const colors = new Float32Array(count * 3)
const velocities = new Float32Array(count * 3)

for (let i = 0; i < count; i++) {
  positions[i * 3] = (Math.random() - 0.5) * 20
  positions[i * 3 + 1] = (Math.random() - 0.5) * 20
  positions[i * 3 + 2] = (Math.random() - 0.5) * 20
  
  colors[i * 3] = 0.5 + Math.random() * 0.5
  colors[i * 3 + 1] = 0.2 + Math.random() * 0.3
  colors[i * 3 + 2] = 0.8 + Math.random() * 0.2
  
  velocities[i * 3] = (Math.random() - 0.5) * 0.5
  velocities[i * 3 + 1] = (Math.random() - 0.5) * 0.5
  velocities[i * 3 + 2] = (Math.random() - 0.5) * 0.5
}

const pointsRef = ref()
const geometryRef = ref()

const { onBeforeRender } = useRenderLoop()
onBeforeRender(({ elapsed, delta }) => {
  if (pointsRef.value && geometryRef.value) {
    pointsRef.value.rotation.y = elapsed * 0.05
    pointsRef.value.rotation.z = elapsed * 0.03
    
    const attr = geometryRef.value.attributes.position
    
    for (let i = 0; i < count; i++) {
      const x = attr.getX(i)
      const y = attr.getY(i)
      const z = attr.getZ(i)
      
      // 应用速度
      let newX = x + velocities[i * 3] * delta
      let newY = y + velocities[i * 3 + 1] * delta
      let newZ = z + velocities[i * 3 + 2] * delta
      
      // 边界反弹
      if (Math.abs(newX) > 10) velocities[i * 3] *= -1
      if (Math.abs(newY) > 10) velocities[i * 3 + 1] *= -1
      if (Math.abs(newZ) > 10) velocities[i * 3 + 2] *= -1
      
      // 正弦波动画
      newY += Math.sin(elapsed + x) * 0.02
      
      attr.setXYZ(i, newX, newY, newZ)
    }
    attr.needsUpdate = true
  }
})
</script>

<template>
  <TresPoints ref="pointsRef">
    <TresBufferGeometry ref="geometryRef">
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
      :size="0.15"
      :vertex-colors="true"
      :transparent="true"
      :opacity="0.9"
      :size-attenuation="true"
    />
  </TresPoints>
</template>
