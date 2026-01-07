<script setup lang="ts">
import { ref } from 'vue'
import Experience from './components/Experience.vue'
import { 
  CircleDot, 
  Box, 
  Wind, 
  Sparkles,
  Info
} from 'lucide-vue-next'

const modes = [
  { id: 'bubbles', name: '解压气泡', icon: CircleDot, color: 'bg-pink-500', desc: '点击破裂气泡' },
  { id: 'gravity', name: '重力方块', icon: Box, color: 'bg-blue-500', desc: '点击生成方块' },
  { id: 'zen', name: '禅意空间', icon: Wind, color: 'bg-emerald-500', desc: '观赏旋转律动' },
  { id: 'sparkles', name: '梦幻星尘', icon: Sparkles, color: 'bg-purple-500', desc: '粒子流动盛宴' },
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
              'flex flex-col items-center justify-center w-20 h-20 md:w-24 md:h-24 rounded-xl transition-all duration-300 gap-2 group relative',
              currentMode === mode.id 
                ? `${mode.color} text-white scale-105 shadow-lg` 
                : 'hover:bg-white/5 text-slate-400'
            ]"
            :title="mode.desc"
          >
            <component :is="mode.icon" class="w-6 h-6 md:w-8 md:h-8" />
            <span class="text-[10px] md:text-xs font-medium">{{ mode.name }}</span>
            
            <!-- 悬停提示 -->
            <div class="absolute bottom-full mb-2 px-2 py-1 bg-black/80 text-white text-xs rounded whitespace-nowrap opacity-0 group-hover:opacity-100 transition-opacity pointer-events-none">
              {{ mode.desc }}
            </div>
          </button>
        </div>
      </nav>

      <!-- Footer / Instructions -->
      <footer class="flex flex-col items-center gap-3 pointer-events-none">
        <div class="bg-white/5 backdrop-blur-sm px-4 py-2 rounded-full border border-white/5 text-[10px] md:text-xs text-slate-500 uppercase tracking-widest">
          🖱️ 鼠标拖拽旋转 • 🔄 滚轮缩放 • 📍 点击互动
        </div>
        <div class="text-center text-[10px] md:text-xs text-slate-600">
          当前模式: <span class="text-slate-400">{{ modes.find(m => m.id === currentMode)?.name }}</span>
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
        <div class="bg-slate-900 border border-white/10 p-8 rounded-3xl max-w-md shadow-2xl max-h-[80vh] overflow-y-auto">
          <h2 class="text-2xl font-bold mb-4">🎮 游戏玩法</h2>
          <p class="text-slate-400 mb-6 leading-relaxed">
            欢迎来到 Zen Space！这是一个专门为你设计的 3D 解压空间。选择你喜欢的模式，通过简单的互动释放压力。
          </p>
          
          <div class="space-y-4 mb-8">
            <div class="bg-slate-800/50 p-4 rounded-lg border border-pink-500/30">
              <h3 class="text-pink-400 font-bold mb-2">💗 解压气泡</h3>
              <p class="text-slate-300 text-sm">尽情点击那些彩色的透明球体，看它们在破裂动画中消失，然后自动重生。享受破裂的快感！</p>
            </div>
            
            <div class="bg-slate-800/50 p-4 rounded-lg border border-blue-500/30">
              <h3 class="text-blue-400 font-bold mb-2">🎲 重力方块</h3>
              <p class="text-slate-300 text-sm">点击屏幕任意位置生成蓝色方块，观看它们在重力作用下自然堆叠和碰撞。体验物理的魅力！</p>
            </div>
            
            <div class="bg-slate-800/50 p-4 rounded-lg border border-emerald-500/30">
              <h3 class="text-emerald-400 font-bold mb-2">🌿 禅意空间</h3>
              <p class="text-slate-300 text-sm">沉浸在绿色的旋转几何体和脉冲装饰中，感受极简设计的宁静与和谐。冥想与放松的完美结合。</p>
            </div>
            
            <div class="bg-slate-800/50 p-4 rounded-lg border border-purple-500/30">
              <h3 class="text-purple-400 font-bold mb-2">✨ 梦幻星尘</h3>
              <p class="text-slate-300 text-sm">数千个紫色粒子在空间中流动舞动，随你的视角而变化。沉浸在梦幻的粒子盛宴中！</p>
            </div>
          </div>

          <div class="bg-slate-800/50 p-4 rounded-lg mb-6">
            <h3 class="text-slate-300 font-bold mb-2">🎮 通用控制</h3>
            <ul class="text-slate-400 text-sm space-y-1">
              <li>🖱️ <strong>鼠标拖拽</strong>：旋转视角</li>
              <li>🔄 <strong>滚轮</strong>：缩放场景</li>
              <li>📍 <strong>点击</strong>：与物体互动</li>
            </ul>
          </div>

          <button 
            @click="showInfo = false"
            class="w-full py-3 bg-gradient-to-r from-pink-500 to-purple-500 text-white font-bold rounded-xl hover:shadow-lg transition-all"
          >
            开始体验 ✨
          </button>
        </div>
      </div>
    </Transition>
  </div>
</template>
