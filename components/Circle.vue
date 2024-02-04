<script setup lang="ts">
const props = defineProps<{
  position?: { x: number, y: number }
}>();

const circle = ref<HTMLElement | null>();
const scale = ref(1);

const getCenter = (element: HTMLElement) => {
  const { left, top, width, height } = element.getBoundingClientRect();

  return {
    x: left + width / 2,
    y: top + height / 2,
  };
};

const lerp = (x: number, y: number, a: number) => x * (1 - a) + y * a;
const clamp = (a: number, min = 0, max = 1) => Math.min(max, Math.max(min, a));
const invlerp = (x: number, y: number, a: number) => clamp((a - x) / (y - x));
const range = (
  x1: number,
  y1: number,
  x2: number,
  y2: number,
  a: number
) => lerp(x2, y2, invlerp(x1, y1, a));

watch(
  () => props.position,
  () => {
    if (!props.position || !circle.value) return;
    const elementPosition = getCenter(circle.value);

    const dist = Math.hypot(
      props.position.x - elementPosition.x,
      props.position.y - elementPosition.y
    );

    scale.value = range(0, 150, 1.5, 1, dist);
  }
);
</script>

<template>
  <div
    ref="circle"
    class="grid place-content-center size-6 bg-red-500 text-white rounded-full transition-all"
    :style="{
      scale
    }"
  />
</template>
