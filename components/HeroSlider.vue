<script setup lang="ts">
const props = withDefaults(
  defineProps<{
    images?: string[]
  }>(),
  {
    images: undefined,
  }
)

const { trimImgSrc } = useImgUtils()

const { counter, reset, pause, resume } = useInterval(4000, {
  immediate: true,
  controls: true,
})
watch(counter, (iteration) => {
  if (!props.images || !props.images.length) {
    pause()
  } else if (iteration == props.images.length) {
    reset()
  }
})
</script>

<template>
  <div
    class="bg-blue-500 relative flex min-h-[100vw] flex-col transition-all before:absolute before:bottom-0 before:z-10 before:block before:h-full before:w-full before:bg-gradient-to-t before:from-green-50/60 after:absolute after:-bottom-0.5 after:z-30 after:block after:h-2 after:w-full after:bg-[url(~/assets/svg/line-black.svg)] after:bg-[length:100%_100%] md:min-h-[calc(100dvh-200px)] md:before:via-transparent"
  >
    <TransitionGroup v-if="images && images.length">
      <figure
        v-for="(src, index) in images"
        :key="src"
        v-show="counter == index"
        class="absolute inset-0 h-full w-full overflow-hidden"
      >
        <NuxtImg
          class="h-full w-full origin-bottom animate-zoom-in-once object-cover will-change-transform"
          :placeholder="[1920, 1920, 10]"
          :src="trimImgSrc(src)"
          height="1920"
          provider="cloudinary"
          width="1920"
        />
      </figure>
    </TransitionGroup>
    <div class="relative z-40 flex h-full w-full grow flex-col justify-end">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.v-enter-active,
.v-leave-active {
  transition: opacity 700ms ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}
</style>
