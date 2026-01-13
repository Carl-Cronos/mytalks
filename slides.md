---
layout: center
highlighter: shiki
css: unocss
colorSchema: dark
transition: fade-out
title: Style Guide
exportFilename: Style Guide
lineNumbers: false
drawings:
  persist: false
mdc: true
clicks: 0
preload: false
# glowSeed: 26
glowSeed: 228
routerMode: hash
---

# Style Guide

Shaojiang, Kaka, Jack, Daniele @ 2025/11/13

---
clicks: 4
---

# Styling Issues in the current App

<div class="mt-8 text-left space-y-10 relative">
  <transition name="issues-fade" mode="out-in">
    <div v-if="$clicks === 0" key="empty"></div>
    <div v-else-if="$clicks === 1" key="messy" class="space-y-4">
      <div class="text-2xl text-white">1. Messy theming</div>
```ts
const CardHeader = styled.div<CardHeaderProps>`
  background: ${({ theme, variant = "default" }) => theme.card.cardHeaderBackground[variant]};
  border-radius: ${({ theme }) => `${theme.radii.card} ${theme.radii.card} 0 0`};
  ${space}
`;
```
```tsx
const StyledMenuItem = styled.a<StyledMenuItemProps>`
${({ disabled, theme }) =>
    disabled &&
    css`
      pointer-events: none;
      color: ${theme.isDark ? theme.colors.darkGrey : theme.colors.grey};
    `}
`;
```
    </div>
    <div v-else-if="$clicks === 2" key="adhoc" class="space-y-4">
      <div class="text-2xl text-white">2. Ad-hoc style values</div>
```tsx {1,2,5,9}
<Flex alignItems="center" gap="4px">
  <Icon height="16px" width="16px">
    <CnsStarIcon />
  </Icon>
  <Text fontSize="13px" fontWeight="700">
    {t("What's an Agent Wallet?")}
  </Text>
</Flex>
<Text fontSize="13px" fontWeight="450" color="textSubtle">
  {t("More features coming soon.")}
</Text>
```
```tsx {1,2,3,7}
<Row my="16px">
  <Flex mt="3px">
    <Text color="textSubtle" fontSize="13px">
      {t('Deposited Tokens')}
    </Text>
  </Flex>
  <Text ml="0.5rem">
    {pair && CurrencyAmount.fromRawAmount(pair.token1, token1Supplied.toFixed(0)).toSignificant()}
  </Text>
</Row>
```
    </div>
    <div v-else-if="$clicks === 3" key="type-safe" class="space-y-4">
      <div class="text-2xl text-white">3. Not type safe</div>
```tsx
const StyledCardRibbon = styled.div<Partial<StyledCardRibbonProps>>`
  z-index: 10;
  background-color: ${({ variantColor = "secondary", theme }) => theme.colors[variantColor]};
  position: absolute;
  right: ${({ ribbonPosition }) => (ribbonPosition === "right" ? 0 : "auto")};
  transform: translateX(30%) translateY(0%) rotate(45deg);
  transform: ${({ ribbonPosition }) =>
    ribbonPosition === "right"
      ? "translateX(30%) translateY(0%) rotate(45deg)"
      : "translateX(0%) translateY(200%) rotate(-45deg)"};
  transform-origin: top left;
  width: 96px;
```
```tsx
const PromotedGradient = keyframes`
  0% {
    background-position: 50% 0%;
  }
  50% {
    background-position: 50% 100%;
  }
  100% {
    background-position: 50% 0%;
  }
`;
```
    </div>
    <div v-else-if="$clicks === 4" key="component-variants" class="space-y-4">
      <div class="text-2xl text-white">4. Component variants not organized</div>
```tsx {1,6,11,15,20}
export const LightCard = styled(Card)`
  border: 1px solid ${({ theme }) => theme.colors.backgroundAlt2};
  background-color: ${({ theme }) => theme.colors.backgroundAlt};
`

export const LightGreyCard = styled(Card)`
  border: 1px solid ${({ theme }) => theme.colors.cardBorder};
  background-color: ${({ theme }) => theme.colors.background};
`

export const GreyCard = styled(Card)`
  background-color: ${({ theme }) => theme.colors.dropdown};
`

export const LightTertiaryCard = styled(Card)<{ active: boolean }>`
  border: 2px solid ${({ theme, active }) => (active ? theme.colors.primary : theme.colors.cardBorder)};
  background-color: ${({ theme }) => theme.colors.boostBackground};
`

export const LightBlueCard = styled(Card)`
  background-color: ${({ theme }) => theme.colors.inputBackground};
`

export const DisableCard = styled(Card)`
  border: 1px solid ${({ theme }) => theme.colors.cardBorder};
  background-color: ${({ theme }) => theme.colors.disabled};
`
```
    </div>
  </transition>
</div>

<style>
.issues-fade-enter-active,
.issues-fade-leave-active {
  transition: opacity 400ms ease, transform 400ms ease;
}
.issues-fade-enter-from,
.issues-fade-leave-to {
  opacity: 0;
  transform: translateY(12px);
}
</style>

---

# Style Guide

<v-clicks class="text-2xl leading-relaxed mt-6 space-y-3">

- A fine-grained and well-defined set of tokens (variables) shared by Designers and Developers
- A set of rules followed by Designers and Developers when doing a new project
- Designers: apply variables EVERYWHERE: components, pages. Never use ad-hoc values
- Developers: apply variables EVERYWHERE: css, js, components, pages. Never use ad-hoc values

</v-clicks>

---
clicks: 3
---

# Style Guide - for designers

<div class="relative w-full h-[720px] flex items-center justify-center overflow-hidden mt-8">
  <transition-group name="fade-slide" tag="div">
    <div
      v-if="$clicks >= 1"
      key="figma-variables-colors"
      class="absolute left-0 -top-30 flex w-full h-full items-center justify-center gap-6 px-6"
    >
      <img
        src="/figma-variables.png"
        class=" w-1/2 object-contain rounded-xl shadow-lg border border-white/10"
      />
      <img
        src="/figma-colors.png"
        class=" w-1/2 object-contain rounded-xl shadow-lg border border-white/10"
      />
    </div>
    <img
      v-if="$clicks >= 2"
      key="figma-components"
      src="/figma-components.png"
      class="absolute left-0 top-0 max-h-[680px] max-w-[100%] rounded-xl shadow-lg border border-white/10 object-contain"
    />
    <img
      v-if="$clicks >= 3"
      key="figma-page"
      src="/figma-page.png"
      class="absolute left-0 top-0 max-h-[680px] max-w-[100%] rounded-xl shadow-lg border border-white/10 object-contain"
    />
  </transition-group>
</div>

<style>
.fade-slide-enter-active,
.fade-slide-leave-active {
  transition: all 500ms ease;
}
.fade-slide-enter-from,
.fade-slide-leave-to {
  opacity: 0;
  transform: translateX(40px) scale(0.96);
}
</style>

---
clicks: 0
---

# Style Guide for designers - switch theme

<div class="mt-8 flex items-center justify-center">
  <video
    controls
    src="/figma-change-theme.mov"
    class="w-full max-h-[600px] rounded-xl shadow-2xl border border-white/10"
  ></video>
</div>

---
clicks: 1
---

# Style Guide for developers - tokens

<div class="relative w-full h-[720px] mt-8 flex items-center justify-center overflow-hidden">
  <div class="flex w-full h-full items-center gap-10">
    <div class="w-1/2 h-full flex items-start justify-center">
      <img
        src="/figma-variables.png"
        class="max-h-[680px] w-full max-w-[640px] object-contain rounded-xl shadow-lg border border-white/10"
      />
    </div>
    <div class="w-1/2 h-full flex items-start justify-center">
      <transition name="dev-fade">
        <template v-if="$clicks >= 1">
```ts
import { defineTokens } from "@pandacss/dev";

const tokens = defineTokens.colors({
  aqua: {
    100: { value: "#D0FCFF" },
    200: { value: "#98F5FB" },
    400: { value: "#08F1FF" },
    500: { value: "#00D3E5" },
    700: { value: "#009CB4" },
    900: { value: "#006E80" },
    alpha40: { value: "rgba(8, 241, 255, 0.4)" },
  },
  accent: {
    babyBlue: { value: "#C5E2FF" },
    blue: { value: "#4CA9FF" },
    violet: { value: "#6253FF" },
    aquamarine: { value: "#35FDE3" },
    magenta: { value: "#FB00FF" },
    pink: { value: "#FFCCE7" },
  },
});

export const brand = {
  name: "brand",
  tokens,
};
```
        </template>
      </transition>
    </div>
  </div>
</div>

<style>
.dev-fade-enter-active,
.dev-fade-leave-active {
  transition: all 450ms ease;
}
.dev-fade-enter-from,
.dev-fade-leave-to {
  opacity: 0;
  transform: translateX(24px) scale(0.96);
}
</style>

---
clicks: 1
---

# Style Guide for developers - semantic tokens

<div class="relative w-full h-[720px] mt-8 flex items-center justify-center overflow-hidden">
  <div class="flex w-full h-full items-center gap-10">
    <div class="w-1/2 h-full flex items-start justify-center">
      <img
        src="/figma-colors.png"
        class="max-h-[680px] w-full max-w-[640px] object-contain rounded-xl shadow-lg border border-white/10"
      />
    </div>
    <div class="w-1/2 h-full flex items-start justify-center">
      <transition name="dev-fade">
        <template v-if="$clicks >= 1">
```ts {8-9,14-15}
import { defineSemanticTokens } from "@pandacss/dev";

export const colors = defineSemanticTokens.colors({
  surface: {
    neutral: {
      primary: {
        value: {
          _light: "{colors.neutral.20}",
          _dark: "{colors.neutral.950}",
        },
      },
      secondary: {
        value: {
          _light: "{colors.neutral.50}",
          _dark: "{colors.neutral.800}",
        },
      },
      // ...
    },
  },
  fg: {
    // ...
  },
  border: {
    // ...
  },
  text: {
    // ...
  },
  status: {
    // ...
  }
});
```
        </template>
      </transition>
    </div>
  </div>
</div>

<style>
.dev-fade-enter-active,
.dev-fade-leave-active {
  transition: all 450ms ease;
}
.dev-fade-enter-from,
.dev-fade-leave-to {
  opacity: 0;
  transform: translateX(24px) scale(0.96);
}
</style>

---
clicks: 1
---

# Style Guide for developers - recipes

<div class="relative w-full h-[720px] mt-8 flex items-center justify-center overflow-hidden">
  <div class="flex w-full h-full items-center gap-10">
    <div class="w-1/2 h-full flex items-start justify-center">
      <img
        src="/figma-components.png"
        class="max-h-[680px] w-full max-w-[640px] object-contain rounded-xl shadow-lg border border-white/10"
      />
    </div>
    <div class="w-1/2 h-full flex items-start justify-center">
      <transition name="dev-fade">
        <template v-if="$clicks >= 1">
```ts {8-9,11,17,18,21,22}
export const button = defineRecipe({
  className: "button",
  jsx: ["Button", "IconButton", "SubmitButton"],
  // ...,
  variants: {
    variant: {
      solid: {
        background: "fg.accent.primary",
        color: "fg.onAccent.primary",
        _hover: {
          background: "fg.accent.hoverPrimary",
        },
        _focusVisible: {
          // ...
        },
        _disabled: {
          color: "white",
          background: "bg.disabled",
          cursor: "not-allowed",
          _hover: {
            color: "white",
            background: "bg.disabled",
          },
        },
      },
    }
  }
});
```
        </template>
      </transition>
    </div>
  </div>
</div>

<style>
.dev-fade-enter-active,
.dev-fade-leave-active {
  transition: all 450ms ease;
}
.dev-fade-enter-from,
.dev-fade-leave-to {
  opacity: 0;
  transform: translateX(24px) scale(0.96);
}
</style>

---
clicks: 1
---

# Style Guide for developers - page

<div class="relative w-full h-[720px] mt-8 flex items-center justify-center overflow-hidden">
  <div class="flex w-full h-full items-center gap-10">
    <div class="w-2/3 h-full flex items-start justify-center">
      <img
        src="/figma-page.png"
        class="max-h-[680px] w-full max-w-[640px] object-contain rounded-xl shadow-lg border border-white/10"
      />
    </div>
    <div class="w-1/3 h-full flex items-start justify-center">
      <transition name="dev-fade">
        <template v-if="$clicks >= 1">
```ts {5,6,7,9,10,13}
<Box
   display="flex"
   justifyContent="space-between"
   alignItems="center"
   p="2"
   bg="surface.neutral.default"
   color="text.primary"
   _hover={{
     bg: "surface.active.primary",
     borderRadius: "md",
   }}
>
  <Button variant="solid">
    Click
  </Button>
  ...
</Box>
```
        </template>
      </transition>
    </div>
  </div>
</div>

<style>
.dev-fade-enter-active,
.dev-fade-leave-active {
  transition: all 450ms ease;
}
.dev-fade-enter-from,
.dev-fade-leave-to {
  opacity: 0;
  transform: translateX(24px) scale(0.96);
}
</style>

---
clicks: 0
---

# Style Guide for developers - switch theme

<div class="mt-8 flex items-center justify-center">
  <video
    controls
    src="/dev-change-theme.mov"
    class="w-full max-h-[600px] rounded-xl shadow-2xl border border-white/10"
  ></video>
</div>

---
clicks: 3
---

# Design Tokens - Colors

<div class="mt-8 relative w-full">
  <!-- Original color grid - scales and moves left on click 2 -->
  <div
    class="flex flex-row gap-1 transition-all duration-700 ease-out"
    :class="$clicks >= 2 ? 'scale-[0.6] justify-start origin-left' : 'justify-center'"
  >
    <ColorVectors
      color="gray"
      :highlighted-shades="$clicks >= 1 ? [100, 400, 500, 900] : []"
      :is-highlighted="$clicks >= 1"
    />
    <ColorVectors
      color="red"
      :highlighted-shades="$clicks >= 1 ? [600] : []"
      :is-highlighted="$clicks >= 1"
    />
    <ColorVectors
      color="blue"
      :highlighted-shades="$clicks >= 1 ? [500] : []"
      :is-highlighted="$clicks >= 1"
    />
    <ColorVectors
      color="green"
      :highlighted-shades="$clicks >= 1 ? [700] : []"
      :is-highlighted="$clicks >= 1"
    />
    <ColorVectors
      color="orange"
      :highlighted-shades="$clicks >= 1 ? [600] : []"
      :is-highlighted="$clicks >= 1"
    />
    <ColorVectors color="yellow" :highlighted-shades="[]" :is-highlighted="$clicks >= 1" />
    <ColorVectors color="purple" :highlighted-shades="[]" :is-highlighted="$clicks >= 1" />
    <ColorVectors
      color="pink"
      :highlighted-shades="$clicks >= 1 ? [500] : []"
      :is-highlighted="$clicks >= 1"
    />
    <ColorVectors
      color="teal"
      :highlighted-shades="$clicks >= 1 ? [100] : []"
      :is-highlighted="$clicks >= 1"
    />
  </div>

  <!-- Semantic vectors on the right - appear on click 2 -->
  <div
    v-if="$clicks >= 2"
    class="absolute top-0 right-0 flex flex-col items-center transition-all duration-700 ease-out"
    :class="$clicks >= 2 ? 'opacity-100 translate-x-0' : 'opacity-0 translate-x-20'"
  >
    <h2 class="text-xl font-semibold mb-4 text-white">Semantic Tokens</h2>
    <div class="flex flex-row gap-1">
      <SemanticVector
        title="status"
        :items="[
          { color: 'red', shade: 600, label: 'error' },
          { color: 'blue', shade: 500, label: 'info' },
          { color: 'orange', shade: 600, label: 'warning' },
          { color: 'green', shade: 700, label: 'success' },
        ]"
        :is-visible="$clicks >= 2"
        :should-transform="$clicks >= 3"
      />
      <SemanticVector
        title="text"
        :items="[
          { color: 'gray', shade: 900, label: 'primary' },
          { color: 'gray', shade: 500, label: 'secondary' },
          { color: 'gray', shade: 400, label: 'tertiary' },
          { color: 'red', shade: 600, label: 'highlight' },
        ]"
        :is-visible="$clicks >= 2"
        :should-transform="$clicks >= 3"
      />
      <SemanticVector
        title="bg"
        :items="[
          { color: 'gray', shade: 100, label: 'neutral' },
          { color: 'teal', shade: 100, label: 'card' },
          { color: 'pink', shade: 500, label: 'emphasized' },
        ]"
        :is-visible="$clicks >= 2"
        :should-transform="$clicks >= 3"
      />
    </div>
  </div>
</div>

---
clicks: 2
---

# Semantic Tokens -> Code

<div class="mt-4 flex gap-4 items-start">
  <div class="flex flex-col items-start">
    <div class="flex flex-row gap-1 pt-20">
      <SemanticVector
        title="status"
        :items="[
          { color: 'red', shade: 600, label: 'error' },
          { color: 'blue', shade: 500, label: 'info' },
          { color: 'orange', shade: 600, label: 'warning' },
          { color: 'green', shade: 700, label: 'success' },
        ]"
        :is-visible="true"
        :should-transform="false"
        static-display
      />
      <SemanticVector
        title="text"
        :items="[
          { color: 'gray', shade: 900, label: 'primary' },
          { color: 'gray', shade: 500, label: 'secondary' },
          { color: 'gray', shade: 400, label: 'tertiary' },
          { color: 'red', shade: 600, label: 'highlight' },
        ]"
        :is-visible="true"
        :should-transform="false"
        static-display
      />
      <SemanticVector
        title="bg"
        :items="[
          { color: 'gray', shade: 100, label: 'neutral' },
          { color: 'teal', shade: 100, label: 'card' },
          { color: 'pink', shade: 500, label: 'emphasized' },
        ]"
        :is-visible="true"
        :should-transform="false"
        static-display
      />
    </div>
  </div>
  <div class="flex-1 grid grid-cols-2 gap-2" v-if="$clicks >= 1">

<div class="col-span-2">

```css {2-3}
body {
  color: var(--colors-text-primary);
  background-color: var(--colors-bg-neutral);
}
```

</div>

```ts {5-7}
const button = defineRecipe({
  // ...
  variants: {
    solid: {
      background: 'bg.emphasized',
      color: 'text.primary',
    },
  },
})
```

```tsx {2-3}
<Box
  backgroundColor="status.error"
  color="text.highlight"
>
  Error message
</Box>
```

```tsx {2-3}
<TickIcon
  className={css({
    color: 'status.success'
  })}
/>
```

```tsx {1}
<MyComp bg="bg.card">
  A Card
</MyComp>
```
  </div>
</div>

---
class: flex items-center justify-center
---

# Thank you
