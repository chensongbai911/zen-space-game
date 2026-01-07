<script setup lang="ts">
import { TresCanvas } from '@tresjs/core'
import { OrbitControls, Stars } from '@tresjs/cientos'
import BubblesMode from './modes/BubblesMode.vue'
import GravityMode from './modes/GravityMode.vue'
import ZenMode from './modes/ZenMode.vue'
import SparklesMode from './modes/SparklesMode.vue'

defineProps<{
  mode: string
}>()
</script>

<template>
  <TresCanvas clear-color="#0f172a" :window-size="true">
    <TresPerspectiveCamera :position="[0, 5, 10]" :look-at="[0, 0, 0]" />
    
    <OrbitControls 
      :enable-damping="true" 
      :damping-factor="0.05"
      :enable-zoom="true"
      :max-distance="20"
      :min-distance="5"
    />

    <!-- Lighting -->
    <TresAmbientLight :intensity="0.5" />
    <TresDirectionalLight
      :position="[5, 5, 5]"
      :intensity="1"
    />

    <!-- Environment -->
    <Stars :radius="100" :depth="50" :count="5000" :factor="4" :saturation="0" :fade="true" />
    
    <!-- Modes -->
    <BubblesMode v-if="mode === 'bubbles'" />
    <GravityMode v-if="mode === 'gravity'" />
    <ZenMode v-if="mode === 'zen'" />
    <SparklesMode v-if="mode === 'sparkles'" />
  </TresCanvas>
</template>
