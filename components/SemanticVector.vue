<script setup lang="ts">
import { computed, ref, watch } from 'vue'
import { getColorHex, getTextColorForBg } from './ColorVectors.vue'

const props = defineProps<{
  title: string
  items: Array<{ color: string, shade: number, label: string }>
  isVisible?: boolean
  shouldTransform?: boolean
  staticDisplay?: boolean
}>()

// Track animation state for each item
const itemStates = ref<Record<string, 'shade' | 'source-text' | 'source-below' | 'label'>>({})

// Track box text state
const boxTextStates = ref<Record<string, 'shade' | 'source'>>({})

watch(
  () => props.isVisible,
  (visible) => {
    if (!visible) return
    props.items.forEach((item, index) => {
      const key = `${item.color}-${item.shade}`
      if (props.staticDisplay) {
        itemStates.value[key] = 'label'
        boxTextStates.value[key] = 'source'
        return
      }
      // Start with shade number after flying
      itemStates.value[key] = 'shade'
      boxTextStates.value[key] = 'shade'

      // After flying completes, change text to color.shade
      setTimeout(() => {
        // avoid running if component switched to static
        if (props.staticDisplay) return
        boxTextStates.value[key] = 'source'
        itemStates.value[key] = 'source-text'
      }, index * 300 + 1000)
    })
  },
  { immediate: true }
)

watch(
  () => props.shouldTransform,
  (shouldTransform) => {
    if (!shouldTransform || props.staticDisplay) return
    props.items.forEach((item, index) => {
      const key = `${item.color}-${item.shade}`
      // Move source text down below box
      setTimeout(() => {
        if (props.staticDisplay) return
        itemStates.value[key] = 'source-below'
      }, index * 100)

      // Then show label in box
      setTimeout(() => {
        if (props.staticDisplay) return
        itemStates.value[key] = 'label'
      }, index * 100 + 400)
    })
  },
  { immediate: true }
)

// Re-export color functions (we'll need to import them properly)
function getColorHexLocal(color: string, shade: number) {
  const colorMap: Record<string, Record<number, string>> = {
    gray: { 100: '#f3f4f6', 400: '#9ca3af', 500: '#6b7280', 900: '#111827' },
    red: { 600: '#dc2626' },
    blue: { 500: '#3b82f6' },
    orange: { 600: '#ea580c' },
    green: { 700: '#15803d' },
    teal: { 100: '#ccfbf1' },
    pink: { 500: '#ec4899' },
  }
  return colorMap[color]?.[shade] || '#000000'
}

function getTextColorForBgLocal(hex: string): string {
  function hexToRgb(hex: string): { r: number, g: number, b: number } | null {
    const result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex)
    return result
      ? {
          r: Number.parseInt(result[1], 16),
          g: Number.parseInt(result[2], 16),
          b: Number.parseInt(result[3], 16),
        }
      : null
  }
  
  function getLuminance(hex: string): number {
    const rgb = hexToRgb(hex)
    if (!rgb) return 0
    const [r, g, b] = [rgb.r, rgb.g, rgb.b].map((val) => {
      val = val / 255
      return val <= 0.03928 ? val / 12.92 : ((val + 0.055) / 1.055) ** 2.4
    })
    return 0.2126 * r + 0.7152 * g + 0.0722 * b
  }
  
  const luminance = getLuminance(hex)
  return luminance > 0.5 ? '#000000' : '#ffffff'
}

function getTextColor(title: string) {
  const colorMap: Record<string, string> = {
    status: '#ffffff',
    text: '#ffffff',
    bg: '#ffffff',
  }
  return colorMap[title] || '#ffffff'
}
</script>

<template>
  <div class="flex flex-col" v-if="isVisible">
    <!-- Title -->
    <div class="flex items-center justify-center mb-4">
      <span
        class="border-2 rounded-md border-solid px-2 py-1 text-sm capitalize"
        :style="{
          color: getTextColor(title),
          borderColor: getTextColor(title),
        }"
      >
        {{ title }}
      </span>
    </div>

    <!-- Vertical vector with semantic boxes -->
    <div class="vertical-vector relative mx-3 flex flex-col px-1">
      <div
        v-for="(item, index) in items"
        :key="`${item.color}-${item.shade}`"
        class="py-1 flex flex-col items-center justify-center"
        style="min-height: 3rem;"
      >
        <!-- Fixed position container for box - prevents shifting -->
        <div class="relative w-16 flex flex-col items-center justify-center" style="min-height: 2rem;">
          <!-- Box that stays in place -->
          <div
            class="w-16 h-8 rounded border border-gray-600/20 flex items-center justify-center text-xs font-medium transition-all duration-300"
            :style="{
              backgroundColor: getColorHexLocal(item.color, item.shade),
              color: getTextColorForBgLocal(getColorHexLocal(item.color, item.shade)),
            }"
            :class="{
              'animate-fly-and-stop': isVisible && itemStates[`${item.color}-${item.shade}`] === 'shade',
            }"
          >
            <!-- Show shade number initially, then color.shade, then label -->
            <span v-if="itemStates[`${item.color}-${item.shade}`] === 'shade' || itemStates[`${item.color}-${item.shade}`] === 'source-text'">
              <span v-if="boxTextStates[`${item.color}-${item.shade}`] === 'shade' || !boxTextStates[`${item.color}-${item.shade}`]">
                {{ item.shade }}
              </span>
              <span v-else class="text-[10px]">
                {{ item.color }}.{{ item.shade }}
              </span>
            </span>
            <span v-else-if="itemStates[`${item.color}-${item.shade}`] === 'source-below' || itemStates[`${item.color}-${item.shade}`] === 'label'" class="animate-fade-in-label">
              {{ item.label }}
            </span>
          </div>
          
          <!-- Source text below box (appears when moving down) -->
          <div
            v-if="itemStates[`${item.color}-${item.shade}`] === 'source-below' || itemStates[`${item.color}-${item.shade}`] === 'label'"
            class="text-[10px] opacity-60 mt-0.5 text-gray-400 transition-all duration-300"
            :class="itemStates[`${item.color}-${item.shade}`] === 'source-below' ? 'animate-move-down' : 'opacity-60'"
          >
            {{ item.color }}.{{ item.shade }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.vertical-vector::before,
.vertical-vector::after {
  content: '';
  position: absolute;
  top: 0;
  bottom: 0;
  width: 10px;
  border: 1px solid rgb(255, 255, 255);
}

.vertical-vector::before {
  left: -6px;
  border-right: none;
}

.vertical-vector::after {
  right: -6px;
  border-left: none;
}

@keyframes flyAndStop {
  0% {
    opacity: 1;
    transform: translateX(-500px) translateY(0) scale(1);
  }
  40% {
    opacity: 1;
    transform: translateX(0) translateY(0) scale(1);
  }
  100% {
    opacity: 1;
    transform: translateX(0) translateY(0) scale(1);
  }
}

.animate-fly-and-stop {
  animation: flyAndStop 1s ease-out forwards;
  opacity: 0;
}

@keyframes moveDown {
  0% {
    opacity: 0;
    transform: translateY(-8px) scale(1.2);
  }
  100% {
    opacity: 0.6;
    transform: translateY(0) scale(1);
  }
}

.animate-move-down {
  animation: moveDown 0.4s ease-out forwards;
}

@keyframes fadeInLabel {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

.animate-fade-in-label {
  animation: fadeInLabel 0.3s ease-out forwards;
}
</style>

