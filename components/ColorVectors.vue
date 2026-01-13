<script setup lang="ts">
const props = defineProps<{
  color: string
  highlightedShades?: number[]
  isHighlighted?: boolean
  hideBoxes?: number[]
}>()

const shades = [100, 200, 300, 400, 500, 600, 700, 800, 900]

// Tailwind color palette hex values
const colorMap: Record<string, Record<number, string>> = {
  gray: {
    100: '#f3f4f6',
    200: '#e5e7eb',
    300: '#d1d5db',
    400: '#9ca3af',
    500: '#6b7280',
    600: '#4b5563',
    700: '#374151',
    800: '#1f2937',
    900: '#111827',
  },
  red: {
    100: '#fee2e2',
    200: '#fecaca',
    300: '#fca5a5',
    400: '#f87171',
    500: '#ef4444',
    600: '#dc2626',
    700: '#b91c1c',
    800: '#991b1b',
    900: '#7f1d1d',
  },
  blue: {
    100: '#dbeafe',
    200: '#bfdbfe',
    300: '#93c5fd',
    400: '#60a5fa',
    500: '#3b82f6',
    600: '#2563eb',
    700: '#1d4ed8',
    800: '#1e40af',
    900: '#1e3a8a',
  },
  green: {
    100: '#dcfce7',
    200: '#bbf7d0',
    300: '#86efac',
    400: '#4ade80',
    500: '#22c55e',
    600: '#16a34a',
    700: '#15803d',
    800: '#166534',
    900: '#14532d',
  },
  orange: {
    100: '#ffedd5',
    200: '#fed7aa',
    300: '#fdba74',
    400: '#fb923c',
    500: '#f97316',
    600: '#ea580c',
    700: '#c2410c',
    800: '#9a3412',
    900: '#7c2d12',
  },
  yellow: {
    100: '#fef9c3',
    200: '#fef08a',
    300: '#fde047',
    400: '#facc15',
    500: '#eab308',
    600: '#ca8a04',
    700: '#a16207',
    800: '#854d0e',
    900: '#713f12',
  },
  purple: {
    100: '#f3e8ff',
    200: '#e9d5ff',
    300: '#d8b4fe',
    400: '#c084fc',
    500: '#a855f7',
    600: '#9333ea',
    700: '#7e22ce',
    800: '#6b21a8',
    900: '#581c87',
  },
  pink: {
    100: '#fce7f3',
    200: '#fbcfe8',
    300: '#f9a8d4',
    400: '#f472b6',
    500: '#ec4899',
    600: '#db2777',
    700: '#be185d',
    800: '#9f1239',
    900: '#831843',
  },
  teal: {
    100: '#ccfbf1',
    200: '#99f6e4',
    300: '#5eead4',
    400: '#2dd4bf',
    500: '#14b8a6',
    600: '#0d9488',
    700: '#0f766e',
    800: '#115e59',
    900: '#134e4a',
  },
}

function getColorHex(color: string, shade: number) {
  return colorMap[color]?.[shade] || '#000000'
}

function getTextColor(color: string) {
  return colorMap[color]?.[500] || '#6b7280'
}

function getBorderColor(color: string) {
  return colorMap[color]?.[500] || '#6b7280'
}

// Calculate relative luminance to determine if color is light or dark
function getLuminance(hex: string): number {
  const rgb = hexToRgb(hex)
  if (!rgb)
    return 0

  // Calculate relative luminance
  const [r, g, b] = [rgb.r, rgb.g, rgb.b].map((val) => {
    val = val / 255
    return val <= 0.03928 ? val / 12.92 : ((val + 0.055) / 1.055) ** 2.4
  })

  return 0.2126 * r + 0.7152 * g + 0.0722 * b
}

// Convert hex to RGB
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

// Get text color (black or white) based on background luminance
function getTextColorForBg(hex: string): string {
  const luminance = getLuminance(hex)
  // Use black text for light backgrounds (luminance > 0.5), white for dark
  return luminance > 0.5 ? '#000000' : '#ffffff'
}
</script>

<template>
  <div class="flex flex-col">
    <!-- Title -->
    <div class="flex items-center justify-center mb-4">
      <span
        class="border-2 rounded-md border-solid px-2 py-1 text-sm capitalize"
        :style="{
          color: getTextColor(color),
          borderColor: getBorderColor(color),
        }"
      >
        {{ color }}
      </span>
    </div>

    <!-- Vertical vector with color boxes -->
    <div class="vertical-vector relative mx-3 flex flex-col px-1">
      <div
        v-for="shade in shades"
        :key="shade"
        class="py-1 flex items-center justify-center"
      >
        <div
          v-if="!props.hideBoxes?.includes(shade)"
          class="w-16 h-8 rounded border border-gray-600/20 flex items-center justify-center text-xs font-medium transition-all duration-300 ease-out"
          :class="{
            'scale-110 z-10': props.isHighlighted && props.highlightedShades?.includes(shade),
            'opacity-30': props.isHighlighted && (!props.highlightedShades || props.highlightedShades.length === 0 || !props.highlightedShades.includes(shade)),
          }"
          :style="{
            backgroundColor: getColorHex(color, shade),
            color: getTextColorForBg(getColorHex(color, shade)),
          }"
        >
          {{ shade }}
        </div>
        <div
          v-else
          class="w-16 h-8"
        />
      </div>
    </div>
  </div>
</template>

<style>
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
</style>
