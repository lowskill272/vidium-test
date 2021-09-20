<template>
  <div class="swiper">
    <div class="slide">1</div>
    <div class="slide">1</div>
    <div class="slide">1</div>
    <div class="slide">1</div>
    <div class="slide">1</div>
    <div class="slide">1</div>
  </div>
</template>

<script lang="ts">

import { Swiper, SwiperOptions } from "swiper";
import "swiper/swiper-bundle.css";

import { Options, Vue } from "vue-class-component";

@Options({
  components: {

  },
})
export default class Schedule extends Vue {

  swiperParams: SwiperOptions = {
    slidesPerView: 3,
    spaceBetween: 50,
  };

  swiper = new Swiper(".swiper", this.swiperParams);
}
</script>

<style scoped lang="scss">
.swiper {
  width: 600px;
  height: 300px;
}
</style>
