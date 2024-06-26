<script setup lang="ts">
import 'vue-slider-component/theme/antd.css';

import VueSlider, { DefineComponent as TVueSlider } from 'vue-slider-component';

// Note that we are explicitly importing from 'tailwind.config.js' which is a vite alias (see vite.config.ts)
// because we need to use ES modules but tailwind + tailwind intellisense require commonJS import style to work properly.
import tailwindConfig from 'tailwind.config.js';
import useDarkMode from '@/composables/useDarkMode';

export interface BalRangeInputProps extends /* @vue-ignore */ TVueSlider {
  modelValue: number;
  leftLabel?: string;
  rightLabel?: string;
}

const props = withDefaults(defineProps<BalRangeInputProps>(), {
  leftLabel: '',
  rightLabel: '',
});
const emit = defineEmits(['change', 'update:modelValue', 'dragEnd']);

const { darkMode } = useDarkMode();

const colors = tailwindConfig.theme.extend.colors;

function onChange(value) {
  emit('change', value);
  emit('update:modelValue', value);
}

function onDragEnd() {
  emit('dragEnd', props.modelValue);
}

const dotStyle = computed(() => {
  return {
    backgroundColor: colors.blue['500'],
    borderColor: colors.blue['500'],
    borderWidth: 0,
    backgroundImage: `linear-gradient(to top right, #05dbf3, #0468be )`,
  };
});

const railSyle = computed(() => {
  return {
    background: darkMode.value ? colors.gray['900'] : colors.gray['100'],
  };
});

const proccessStyle = computed(() => {
  return {
    backgroundImage: `linear-gradient(to top right, #05dbf3, #0468be )`,
  };
});
</script>

<template>
  <div class="pr-2">
    <div class="flex justify-between text-sm text-secondary">
      <div>
        <slot v-if="$slots.leftLabel || leftLabel" name="leftLabel">
          {{ leftLabel }}
        </slot>
      </div>
      <div>
        <slot v-if="$slots.rightLabel || rightLabel" name="rightLabel">
          {{ rightLabel }}
        </slot>
      </div>
    </div>
    <VueSlider
      :modelValue="modelValue"
      v-bind="$attrs"
      :dotStyle="dotStyle"
      :railStyle="railSyle"
      :processStyle="proccessStyle"
      @change="onChange"
      @drag-end="onDragEnd"
    />
  </div>
</template>

<style>
.vue-slider-dot-handle-focus {
  box-shadow: 0 0 0 5px rgb(0 0 0 / 20%);
}
</style>
