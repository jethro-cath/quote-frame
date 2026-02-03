<template>
  <div class="controls">
    <div class="control-section">
      <div class="heading">Цитата</div>
      <div class="control-group">
        <label>Текст:</label>
        <textarea
          class="text-input input-or-select"
          placeholder="Введите цитату"
          :value="props.quoteText"
          @input="emit('update:quoteText', $event.target.value)"
        />
      </div>

      <div class="control-group">
        <label>Размер шрифта:</label>
        <input
          class="slider"
          type="range"
          min="24"
          max="72"
          :value="props.quoteFontSize"
          @input="emit('update:quoteFontSize', Number($event.target.value))"
        />
      </div>

      <div class="control-group">
        <label>Шрифт:</label>
        <select
          class="input-or-select"
          :value="props.quoteFont.value"
          @change="onQuoteFontChange"
        >
          <option
            v-for="font in fonts"
            :key="font.value"
            :value="font.value"
            :style="{ fontFamily: font.css }"
          >
            {{ font.label }}
          </option>
        </select>
      </div>

      <div class="control-group">
        <p>Выравнивание:</p>
        <div class="align-buttons">
          <button
            class="align-button-item"
            v-for="option in alignOptions"
            :key="option.id"
            :class="{ 'item-selected': quoteAlign === option.id }"
            @click="$emit('update:quoteAlign', option.id)"
          >
            <img :src="option.icon" />
          </button>
        </div>
      </div>
    </div>
    <hr />

    <div class="control-section">
      <div class="heading">Автор</div>
      <div class="control-group">
        <label>Имя автора:</label>
        <textarea
          class="text-input input-or-select"
          placeholder="Введите имя автора"
          :value="props.author"
          @input="emit('update:author', $event.target.value)"
        />
      </div>

      <div class="control-group">
        <label>Размер шрифта:</label>
        <input
          class="slider"
          type="range"
          min="14"
          max="60"
          :value="props.authorFontSize"
          @input="emit('update:authorFontSize', Number($event.target.value))"
        />
      </div>

      <div class="control-group">
        <label>Шрифт:</label>
        <select
          class="input-or-select"
          :value="props.authorFont.value"
          @change="onauthorFontChange"
        >
          <option
            v-for="font in fonts"
            :key="font.value"
            :value="font.value"
            :style="{ fontFamily: font.css }"
          >
            {{ font.label }}
          </option>
        </select>
      </div>

      <div class="control-group">
        <p>Выравнивание:</p>
        <div class="align-buttons">
          <button
            class="align-button-item"
            v-for="option in alignOptions"
            :key="option.id"
            :class="{ 'item-selected': authorAlign === option.id }"
            @click="$emit('update:authorAlign', option.id)"
          >
            <img :src="option.icon" />
          </button>
        </div>
      </div>
    </div>

    <hr />

    <div class="control-section">
      <div class="heading">Оформление</div>
      <div class="control-group">
        <p>Стиль рамки:</p>
        <div class="corner-select">
          <button
            class="corner-preview"
            v-for="corner in corners"
            :key="corner.id"
            :class="{ 'item-selected': selectedCorner?.id === corner.id }"
            @click="emit('update:selectedCorner', corner)"
          >
            <img :src="corner.image" />
          </button>
        </div>
      </div>
    </div>
    <button
      class="button"
      @click="emit('download')"
    >
      Сохранить как картинку
    </button>
  </div>
</template>

<script setup>
const props = defineProps({
  quoteText: String,
  quoteFontSize: Number,
  quoteAlign: String,
  author: String,
  authorFontSize: Number,
  authorAlign: String,
  fonts: Array,
  quoteFont: Object,
  authorFont: Object,
  corners: Array,
  selectedCorner: Object,
})

const emit = defineEmits([
  'update:quoteText',
  'update:quoteFontSize',
  'update:quoteFont',
  'update:quoteAlign',
  'update:author',
  'update:authorFontSize',
  'update:authorFont',
  'update:authorAlign',
  'update:selectedCorner',
  'download',
])

import iconAlignLeft from '@/assets/icons/align-left.svg'
import iconAlignCenter from '@/assets/icons/align-center.svg'
import iconAlignRight from '@/assets/icons/align-right.svg'

const alignOptions = [
  { id: 'left', icon: iconAlignLeft },
  { id: 'center', icon: iconAlignCenter },
  { id: 'right', icon: iconAlignRight },
]

function onQuoteFontChange(event) {
  const selected = props.fonts.find((f) => f.value === event.target.value)
  emit('update:quoteFont', selected)
}

function onauthorFontChange(event) {
  const selected = props.fonts.find((f) => f.value === event.target.value)
  emit('update:authorFont', selected)
}
</script>

<style scoped>
.controls {
  display: flex;
  flex-direction: column;
  gap: 20px;
  height: 100vh;
  min-width: 350px;
  width: 20%;
  padding: var(--space-outer);

  background-color: var(--color-panel);

  overflow: auto;
}

.control-section {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.heading {
  font-size: 1.5em;
  font-family: 'Alegreya', serif;
  font-weight: 600;
}

.control-group {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.text-input {
  min-height: 100px;
  padding: 10px;

  resize: vertical;
}

.align-buttons {
  display: flex;
  gap: 8px;
}

.align-button-item {
  height: 36px;
  width: 36px;
  border-radius: 8px;
  border: none;
  outline: 1px solid black;
  background-color: #fff;
}

.corner-select {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
}

.corner-preview {
  height: 80px;
  width: 80px;
  padding: 8px;
  border: none;
  outline: 1px solid black;
  border-radius: 8px;

  background-color: #fff;
}

.corner-preview img {
  width: 100%;
}

.item-selected {
  outline-width: 3px;
  background-color: var(--color-secondary);
}
</style>
