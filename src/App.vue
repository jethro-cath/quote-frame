<template>
  <div class="app">
    <!-- Панель управления -->
    <Controls
      v-model:quoteText="settings.quoteText"
      v-model:quoteFontSize="settings.quoteFontSize"
      v-model:quoteFont="settings.quoteFont"
      v-model:quoteAlign="settings.quoteAlign"
      v-model:author="settings.author"
      v-model:authorFontSize="settings.authorFontSize"
      v-model:authorFont="settings.authorFont"
      v-model:authorAlign="settings.authorAlign"
      v-model:selectedCorner="settings.selectedCorner"
      :fonts="fonts"
      :corners="corners"
      @download="handleDownload"
    />

    <!-- Превью рамки -->
    <FramePreview
      v-bind="settings"
      ref="previewComp"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { toPng } from 'html-to-image'
import Controls from './components/Controls.vue'
import FramePreview from './components/FramePreview.vue'
import fonts from './data/fonts.js'
import corners from './data/corners.js'

const settings = ref({
  quoteText: '',
  quoteFontSize: 36,
  quoteFont: fonts[0],
  quoteAlign: 'center',
  author: '',
  authorFontSize: 24,
  authorFont: fonts[0],
  authorAlign: 'right',
  selectedCorner: corners[0],
})

const previewComp = ref(null)

const handleDownload = async () => {
  const node = previewComp.value?.frameRef
  if (!node) return

  try {
    await document.fonts.ready

    const dataUrl = await toPng(node, {
      cacheBust: true,
      pixelRatio: 2,
      backgroundColor: '#ffffff',
    })

    const link = document.createElement('a')
    link.download = 'quote-high-res.png'
    link.href = dataUrl
    link.click()
  } catch (err) {
    console.error('Ошибка разрешения:', err)
  }
}

fonts.forEach((font) => {
  document.fonts.load(`1em ${font.css}`)
})
</script>

<style>
/* Стили для контейнера приложения */
.app {
  display: flex;
  gap: var(--space-outer);
  align-items: center;
  overflow: hidden;
  padding-right: var(--space-outer);
}
</style>
