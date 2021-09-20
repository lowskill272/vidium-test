<template>
  <swiper
    :modules="modules"
    :slides-per-view="3"
    :space-between="50"
    navigation
    :pagination="{ clickable: true }"
    :scrollbar="{ draggable: true }"
    @swiper="onSwiper"
    @slideChange="onSlideChange"
  >
    <swiper-slide>Slide 1</swiper-slide>
    <swiper-slide>Slide 2</swiper-slide>
    <swiper-slide>Slide 3</swiper-slide>
    <swiper-slide>Slide 4</swiper-slide>
    <swiper-slide>Slide 5</swiper-slide>
    <swiper-slide>Slide 6</swiper-slide>
    <swiper-slide>Slide 7</swiper-slide>
  </swiper>
</template>

<script lang="ts">

import { Navigation, Pagination, Scrollbar, A11y } from "../../node_modules/swiper";
import { Swiper, SwiperSlide } from "../../node_modules/swiper/vue"
import 'swiper/swiper-bundle.css';

import { Options, Vue } from "vue-class-component";

@Options({
  components: {
    Swiper,
    SwiperSlide,
  },
})
export default class Schedule extends Vue {

  onSwiper = (swiper: typeof Swiper) => {
    console.log(swiper);
  };
  onSlideChange = () => {
    console.log('slide change');
  };

  setup() {
    return {
      modules: [Navigation, Pagination, Scrollbar, A11y],
    };
  }
}
</script>

<style scoped lang="scss">
.swiper {
  width: 600px;
  height: 300px;
}
</style>
