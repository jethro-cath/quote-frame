<template>
  <div class="preview-container">
    <div
      class="frame"
      ref="frameRef"
      :style="frameStyles"
    >
      <img
        :src="selectedCorner?.image"
        class="corner-frame corner-top-left"
      />
      <img
        :src="selectedCorner?.image"
        class="corner-frame corner-top-right"
      />
      <img
        :src="selectedCorner?.image"
        class="corner-frame corner-bottom-left"
      />
      <img
        :src="selectedCorner?.image"
        class="corner-frame corner-bottom-right"
      />
      <!-- Просто выводим текст -->
      <p :style="quoteStyles">
        {{ quoteText || 'Текст цитаты' }}
      </p>

      <p :style="authorStyles">
        {{ author || 'Автор' }}
      </p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { computed } from 'vue'

const props = defineProps({
  quoteText: String,
  quoteFontSize: Number,
  quoteFont: Object,
  quoteAlign: String,
  author: String,
  authorFontSize: Number,
  authorFont: Object,
  authorAlign: String,
  selectedCorner: Object,
})

const quoteStyles = computed(() => {
  return {
    fontSize: props.quoteFontSize + 'px',
    fontFamily: props.quoteFont?.css || 'serif',
    textAlign: props.quoteAlign,
  }
})

const authorStyles = computed(() => {
  return {
    fontSize: props.authorFontSize + 'px',
    fontFamily: props.authorFont?.css || 'serif',
    textAlign: props.authorAlign,
    fontStyle: 'italic',
  }
})

const frameStyles = computed(() => {
  return {
    '--current-font-size': props.quoteFontSize + 'px',
  }
})

const frameRef = ref(null)
defineExpose({ frameRef })
</script>

<style scoped>
.preview-container {
  display: flex;
  justify-content: center;

  width: 100%;
  margin: 0 auto;
}

.frame {
  position: relative;
  min-width: 500px;
  min-height: 300px;
  padding: var(--current-font-size) calc(2 * var(--current-font-size));

  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: calc(0.5 * var(--current-font-size));

  font-family: serif;
  font-size: 24px;

  --b: 4px; /* толщина линии */
  --g: 90px; /* размер «пустого» угла */
  --c: black; /* цвет */

  background:
    linear-gradient(var(--c), var(--c)) top / calc(100% - 2 * var(--g)) var(--b),
    linear-gradient(var(--c), var(--c)) bottom / calc(100% - 2 * var(--g))
      var(--b),
    linear-gradient(var(--c), var(--c)) left / var(--b)
      calc(100% - 2 * var(--g)),
    linear-gradient(var(--c), var(--c)) right / var(--b)
      calc(100% - 2 * var(--g));
  background-repeat: no-repeat;
  background-origin: border-box;
}

.frame p {
  width: 100%;
  white-space: pre-line;
}

.corner-frame {
  position: absolute;
  height: 150px;
  width: 150px;
}

.corner-top-left {
  top: -4px;
  left: -4px;
  transform: rotate(90deg);
}

.corner-top-right {
  top: -4px;
  right: -4px;
  transform: rotate(180deg);
}

.corner-bottom-left {
  bottom: -4px;
  left: -4px;
}

.corner-bottom-right {
  bottom: -4px;
  right: -4px;
  transform: rotate(270deg);
}
</style>
