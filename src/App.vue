<script setup lang="ts">
import { ref } from 'vue'
import Experience from './components/Experience.vue'
import { CircleDot, Box, Wind, Sparkles, Info } from 'lucide-vue-next'

const modes = [
  { id: 'bubbles', name: '解压气泡', icon: CircleDot, color: 'bg-pink-500' },
  { id: 'gravity', name: '重力方块', icon: Box, color: 'bg-blue-500' },
  { id: 'zen', name: '禅意空间', icon: Wind, color: 'bg-emerald-500' },
  { id: 'sparkles', name: '梦幻星尘', icon: Sparkles, color: 'bg-purple-500' },
]

const currentMode = ref('bubbles')
const showInfo = ref(false)
</script>

<template>
  <div class="relative w-full h-full">
    <!-- 3D Canvas -->
    <Experience :mode="currentMode" />

    <!-- UI Overlay -->
    <div class="absolute inset-0 pointer-events-none flex flex-col justify-between p-6 md:p-10">
      <!-- Header -->
      <header class="flex justify-between items-start pointer-events-auto">
        <div>
          <h1 class="text-3xl md:text-4xl font-bold tracking-tighter bg-gradient-to-r from-white to-slate-400 bg-clip-text text-transparent">
            ZEN SPACE
          </h1>
          <p class="text-slate-400 text-sm md:text-base mt-1">超级解压的 3D 互动空间</p>
        </div>
        <button 
          @click="showInfo = !showInfo"
          class="p-2 rounded-full bg-white/10 hover:bg-white/20 transition-colors backdrop-blur-md border border-white/10"
        >
          <Info class="w-6 h-6" />
        </button>
      </header>

      <!-- Mode Switcher -->
      <nav class="flex justify-center pointer-events-auto">
        <div class="flex bg-black/30 backdrop-blur-xl p-2 rounded-2xl border border-white/10 shadow-2xl gap-2">
          <button
            v-for="mode in modes"
            :key="mode.id"
            @click="currentMode = mode.id"
            :class="[
              'flex flex-col items-center justify-center w-20 h-20 md:w-24 md:h-24 rounded-xl transition-all duration-300 gap-2',
              currentMode === mode.id 
                ? `${mode.color} text-white scale-105 shadow-lg` 
                : 'hover:bg-white/5 text-slate-400'
            ]"
          >
            <component :is="mode.icon" class="w-6 h-6 md:w-8 md:h-8" />
            <span class="text-[10px] md:text-xs font-medium">{{ mode.name }}</span>
          </button>
        </div>
      </nav>

      <!-- Footer -->
      <footer class="flex justify-center pointer-events-none">
        <div class="bg-white/5 backdrop-blur-sm px-4 py-2 rounded-full border border-white/5 text-[10px] md:text-xs text-slate-500 uppercase tracking-widest">
          鼠标点击或拖拽以互动 • 感受解压时刻
        </div>
      </footer>
    </div>

    <!-- Info Modal -->
    <Transition
      enter-active-class="transition duration-300 ease-out"
      enter-from-class="opacity-0 scale-95"
      enter-to-class="opacity-100 scale-100"
      leave-active-class="transition duration-200 ease-in"
      leave-from-class="opacity-100 scale-100"
      leave-to-class="opacity-0 scale-95"
    >
      <div v-if="showInfo" class="absolute inset-0 z-50 flex items-center justify-center p-6 bg-slate-950/80 backdrop-blur-md pointer-events-auto">
        <div class="bg-slate-900 border border-white/10 p-8 rounded-3xl max-w-md shadow-2xl">
          <h2 class="text-2xl font-bold mb-4">关于 Zen Space</h2>
          <p class="text-slate-400 mb-6 leading-relaxed">
            这是一个专门为你设计的 3D 解压空间。在这里，你可以通过简单的互动释放压力。
          </p>
          <ul class="space-y-3 mb-8 text-sm text-slate-300">
            <li class="flex items-center gap-2">
              <div class="w-1.5 h-1.5 rounded-full bg-pink-500"></div>
              <span><strong>解压气泡：</strong> 彩色透明球体</span>
            </li>
            <li class="flex items-center gap-2">
              <div class="w-1.5 h-1.5 rounded-full bg-blue-500"></div>
              <span><strong>重力方块：</strong> 蓝色几何体</span>
            </li>
            <li class="flex items-center gap-2">
              <div class="w-1.5 h-1.5 rounded-full bg-emerald-500"></div>
              <span><strong>禅意空间：</strong> 绿色旋转体</span>
            </li>
            <li class="flex items-center gap-2">
              <div class="w-1.5 h-1.5 rounded-full bg-purple-500"></div>
              <span><strong>梦幻星尘：</strong> 紫色粒子</span>
            </li>
          </ul>
          <button 
            @click="showInfo = false"
            class="w-full py-3 bg-white text-slate-950 font-bold rounded-xl hover:bg-slate-200 transition-colors"
          >
            开始体验
          </button>
        </div>
      </div>
    </Transition>
  </div>
</template>
