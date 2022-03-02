<script setup>
import { ref } from "@vue/reactivity"
import { computed } from "vue";
let scrollY = ref(0)
let isActive = computed(() => {
  return scrollY.value > 0
})
window.addEventListener('scroll', () => {
  console.log(window.scrollY)
  if (window.scrollY === 0) {
    scrollY.value = 0
  } else {
    scrollY.value = 1
  }

})
//--------------------------------------------
let currentSliderShowIndex = ref(0)
let nextSliderShowIndex = computed(() => {
  return (currentSliderShowIndex.value + 1) % imgArr.value.length
})
let imgArr = computed(() => {
  return [{ src: 'https://picsum.photos/id/1060/1920/768' }, { src: 'https://picsum.photos/1920/768.jpg' }, { src: 'https://picsum.photos/1920/768.jpg' }]
})
setInterval(() => {
  currentSliderShowIndex.value = (currentSliderShowIndex.value + 1) % imgArr.value.length
}, 15000)

function isShowImgHandler(index) {
  return index === nextSliderShowIndex.value || index === currentSliderShowIndex.value
}
</script>

<template>
  <section class="kv">
    <div class="kv-items" :class="{ active: isActive }">
      <img
        v-for="(img, index) in imgArr"
        :key="img.src"
        :src="img.src"
        :class="{ active: isShowImgHandler(index) }"
        class="animation-kv-slider"
      />
    </div>
  </section>
  <section>{{ nextSliderShowIndex }}</section>
</template>


<style lang="scss">
section {
  width: 100%;
  height: 100vh;
}
.kv {
  position: relative;
  overflow: hidden;
  &-items {
    width: 100%;
    height: 100%;
    position: absolute;
    overflow: hidden;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    transition: all 1s;
  }
  &-items.active {
    width: 640px;
    height: 300px;
    border-radius: 30px;
    border: 10px solid #000;
  }
  .animation-kv-slider {
    position: absolute;
    display: block;
    object-fit: cover;
    object-position: center;
    width: 100%;
    height: 100%;
  }
  .animation-kv-slider.active {
    animation-name: kv-slider;
    animation-fill-mode: forwards;
    animation-timing-function: linear;
    animation-iteration-count: infinite;
  }
}

@keyframes kv-slider {
  0% {
    transform: scale(1.6);
    opacity: 0;
  }

  50% {
    opacity: 1;
  }

  100% {
    transform: scale(1);
    opacity: 0;
  }
}
</style>