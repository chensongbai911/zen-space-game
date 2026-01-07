<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { useRenderLoop } from '@tresjs/core'

interface Box {
  id: number
  position: [number, number, number]
  rotation: [number, number, number]
  velocity: [number, number, number]
  color: string
  size: number
}

const boxes = ref<Box[]>([])
const groundY = 0

const createBox = (): Box => ({
  id: Math.random(),
  position: [
    (Math.random() - 0.5) * 8,
    8 + Math.random() * 2,
    (Math.random() - 0.5) * 8
  ],
  rotation: [Math.random() * Math.PI, Math.random() * Math.PI, Math.random() * Math.PI],
  velocity: [
    (Math.random() - 0.5) * 2,
    0,
    (Math.random() - 0.5) * 2
  ],
  color: `hsl(${200 + Math.random() * 40}, 80%, 60%)`,
  size: Math.random() * 0.6 + 0.4
})

onMounted(() => {
  for (let i = 0; i < 12; i++) {
    boxes.value.push(createBox())
  }
})

const { onBeforeRender } = useRenderLoop()
onBeforeRender(({ delta }) => {
  boxes.value.forEach(box => {
    // 重力模拟
    box.velocity[1] -= 9.8 * delta * 0.3
    
    // 位置更新
    box.position[0] += box.velocity[0] * delta
    box.position[1] += box.velocity[1] * delta
    box.position[2] += box.velocity[2] * delta

    // 旋转
    box.rotation[0] += box.velocity[0] * delta * 0.5
    box.rotation[1] += box.velocity[1] * delta * 0.5
    box.rotation[2] += box.velocity[2] * delta * 0.5

    // 地面碰撞
    if (box.position[1] < groundY + box.size / 2) {
      box.position[1] = groundY + box.size / 2
      box.velocity[1] *= -0.7 // 反弹系数
      
      // 碰撞时添加随机速度
      if (Math.abs(box.velocity[1]) > 0.5) {
        box.velocity[0] += (Math.random() - 0.5) * 1
        box.velocity[2] += (Math.random() - 0.5) * 1
      }
    }

    // 空气阻力
    box.velocity[0] *= 0.98
    box.velocity[2] *= 0.98
  })
})

const handleClick = () => {
  // 点击屏幕生成新方块
  const newBox = createBox()
  boxes.value.push(newBox)
  
  // 限制最多50个方块
  if (boxes.value.length > 50) {
    boxes.value.shift()
  }
}
</script>

<template>
  <TresGroup @click="handleClick">
    <!-- Ground -->
    <TresMesh :rotation="[-Math.PI / 2, 0, 0]" :position="[0, groundY, 0]" receive-shadow>
      <TresPlaneGeometry :args="[20, 20]" />
      <TresMeshStandardMaterial color="#1e293b" />
    </TresMesh>

    <!-- Boxes -->
    <TresMesh
      v-for="box in boxes"
      :key="box.id"
      :position="box.position"
      :rotation="box.rotation"
      cast-shadow
    >
      <TresBoxGeometry :args="[box.size, box.size, box.size]" />
      <TresMeshStandardMaterial :color="box.color" :roughness="0.3" :metalness="0.2" />
    </TresMesh>
  </TresGroup>
</template>
