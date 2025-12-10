<script setup lang="ts">
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import type { IconDefinition } from '@fortawesome/fontawesome-svg-core'
import { MarkdownRender } from 'markstream-vue'

defineProps<{
  content: string
  icon?: IconDefinition | null
  images: string[]
  running: boolean
  clickable?: boolean
  isMarkdown?: boolean
  showBorder?: boolean
}>()

const emit = defineEmits<{
  click: []
}>()
</script>

<template>
  <div class="text-gray-600 dark:text-gray-300 text-sm flex items-center gap-2 py-1" :class="{
    'cursor-pointer hover:opacity-80': clickable,
    'border border-gray-300 dark:border-gray-700 rounded px-2': showBorder,
  }" @click="clickable && emit('click')">
    <div v-if="icon" class="flex-shrink-0 w-4 h-4 flex items-center justify-center icon-container" :class="{
      'icon-pulse': running,
    }">
      <FontAwesomeIcon :icon="icon" />
    </div>
    <div class="flex-1 min-w-0">
      <ClientOnly>
        <MarkdownRender v-if="isMarkdown" :content="content" theme="dark" />
        <span v-else>{{ content }}</span>
      </ClientOnly>
    </div>
    <div v-if="images.length > 0" class="flex flex-row gap-2 flex-shrink-0">
      <img v-for="image in images" :key="image" :src="image" alt="Image" class="w-16 h-16 rounded object-cover">
    </div>
  </div>
</template>

<style scoped>
@keyframes iconPulse {

  0%,
  100% {
    opacity: 1;
  }

  50% {
    opacity: 0.3;
  }
}

.icon-container {
  transition: opacity 0.2s ease-in-out;
}

.icon-pulse {
  animation: iconPulse 1.5s ease-in-out infinite;
}
</style>
