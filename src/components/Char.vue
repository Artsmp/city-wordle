<script lang="ts" setup>
const props = defineProps<{
  text: {
    char: string;
    py: string;
    status: 'right' | 'warning' | 'normal';
  };
}>();

const colorMap = {
  right: {
    text: 'text-green-6',
    bg: 'bg-green-6',
  },
  warning: {
    text: 'text-orange-6',
    bg: 'bg-orange-5',
  },
  normal: {
    text: 'text-white',
    bg: '',
  },
};
</script>

<template>
  <div class="inline-flex flex-col justify-center items-center">
    <div mb-1 :class="colorMap[props.text.status].text">
      {{ props.text.py }}
    </div>
    <div
      w-12
      h-12
      border
      flex
      justify-center
      items-center
      relative
      text-xl
      z-1
      font-bold
      :class="colorMap[props.text.status].bg"
      class="after:border-r after:border-gray-400 after:border-dashed after:h-full after:content-empty after:left-50% after:absolute after:pointer-events-none after:z--1 before:pointer-events-none before:z--1 before:w-full before:border-t before:border-gray-400 before:border-dashed before:content-empty before:left-0% before:absolute">
      {{ props.text.char }}
    </div>
  </div>
</template>
