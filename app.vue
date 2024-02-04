<script setup lang="ts">
const container = ref<HTMLElement | null>(null);
const currentPosition = ref<{ x: number, y: number }>();

const totalSize = ref({
  width: 100,
  height: 100,
})

const selectRandomPoint = () => {
  currentPosition.value = {
    x: Math.random() * totalSize.value.width,
    y: Math.random() * totalSize.value.height,
  }
}

onMounted(() => {
  if (!container.value) return;

  const { width, height } = container.value.getBoundingClientRect();
  totalSize.value = {
    width,
    height
  }

  setInterval(selectRandomPoint, 1000);
})
</script>

<template>
  <div ref="container" class="w-fit p-4">
    <ul class="grid grid-cols-[repeat(10,_minmax(0,_1fr))] gap-4">
      <li v-for="i in 100" :key="i">
        <Circle
          :id="i"
          :position="currentPosition"
        />
      </li>
    </ul>
  </div>
</template>
