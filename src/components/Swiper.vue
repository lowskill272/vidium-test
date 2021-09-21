<template>
  <div class="swiper">
    <div class="slide">1</div>
    <div class="slide">2</div>
    <div class="slide">3</div>
    <div class="slide">4</div>
    <div class="slide">5</div>
    <div class="slide">6</div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  props: {
    swiperData: [],

  }
})
</script>

<style scoped lang="scss">
.swiper {
  width: 600px;
  height: 300px;
}
</style>
