<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { useRenderLoop } from '@tresjs/core'
import gsap from 'gsap'

interface Bubble {
  id: number
  position: [number, number, number]
  scale: number
  color: string
  popped: boolean
  mesh?: any
}

const bubbles = ref<Bubble[]>([])

const createBubble = (id: number): Bubble => ({
  id,
  position: [
    (Math.random() - 0.5) * 10,
    (Math.random() - 0.5) * 6 + 2,
    (Math.random() - 0.5) * 10
  ],
  scale: Math.random() * 0.5 + 0.5,
  color: `hsl(${Math.random() * 360}, 70%, 70%)`,
  popped: false,
})

onMounted(() => {
  for (let i = 0; i < 30; i++) {
    bubbles.value.push(createBubble(i))
  }
})

const pop = (bubble: Bubble, event: any) => {
  if (bubble.popped) return
  
  bubble.popped = true
  
  // 破裂动画
  gsap.to(event.object.scale, {
    x: 1.3,
    y: 1.3,
    z: 1.3,
    duration: 0.15,
    onComplete: () => {
      gsap.to(event.object.scale, {
        x: 0,
        y: 0,
        z: 0,
        duration: 0.25,
        ease: 'back.in',
        onComplete: () => {
          // 2秒后重生
          setTimeout(() => {
            const index = bubbles.value.findIndex(b => b.id === bubble.id)
            if (index !== -1) {
              bubbles.value[index] = createBubble(bubble.id)
            }
          }, 2000)
        }
      })
    }
  })
  
  // 音效反馈（可选）
  playPopSound()
}

const playPopSound = () => {
  // 创建一个简单的音效反馈
  const audioContext = new (window.AudioContext || (window as any).webkitAudioContext)()
  const oscillator = audioContext.createOscillator()
  const gainNode = audioContext.createGain()
  
  oscillator.connect(gainNode)
  gainNode.connect(audioContext.destination)
  
  oscillator.frequency.value = 800
  oscillator.type = 'sine'
  
  gainNode.gain.setValueAtTime(0.3, audioContext.currentTime)
  gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + 0.1)
  
  oscillator.start(audioContext.currentTime)
  oscillator.stop(audioContext.currentTime + 0.1)
}

const { onBeforeRender } = useRenderLoop()
onBeforeRender(({ delta }) => {
  // 轻微浮动动画
  bubbles.value.forEach((b, i) => {
    if (!b.popped) {
      b.position[1] += Math.sin(Date.now() * 0.001 + i) * 0.003
      b.position[0] += Math.cos(Date.now() * 0.0008 + i * 0.5) * 0.002
    }
  })
})
</script>

<template>
  <TresGroup>
    <TresMesh
      v-for="bubble in bubbles"
      :key="bubble.id"
      :position="bubble.position"
      :scale="[bubble.scale, bubble.scale, bubble.scale]"
      @click="(ev) => pop(bubble, ev)"
      class="cursor-pointer"
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
