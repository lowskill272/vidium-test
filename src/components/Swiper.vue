<template>
  <div class="wrapper">
    <swiper-item
      v-for="(item, index) in swiperData"
      :key="index"
      :item_data="item"
      :style="{ transform: 'translate(-' + 100 * currentIndex + '%)' }"
      :class="{ swiping: isMouseDown }"
      @mousemove="mouseMove($event)"
      @mouseup="mouseUp($event)"
      @mousedown="mouseDown($event)"
    />
  </div>
  <button @click="prevSlide">Prev</button>
  <button @click="nextSlide">Next</button>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import SwiperItem from "@/components/SwiperItem.vue";

export default defineComponent({
  name: "Swiper",
  components: {
    SwiperItem,
  },
  props: {
    swiperData: {
      type: Array,
      default() {
        return ["Передан", "пустой", "массив"];
      },
    },
  },
  data() {
    return {
      currentIndex: 0,
      startSwipePosition: 0,
      startSwipeIndex: 0,
      isMouseDown: 0,
    };
  },
  methods: {
    prevSlide() {
      if (this.currentIndex > 0) {
        this.currentIndex--;
      }
      console.log(this.currentIndex);
    },
    nextSlide() {
      if (this.currentIndex < (this as any).swiperData.length - 1) {
        this.currentIndex++;
      }
      console.log(this.currentIndex);
    },
    mouseMove(e: MouseEvent) {
      if (this.isMouseDown) {
        this.currentIndex =
          this.startSwipeIndex + (this.startSwipePosition - e.clientX) / 300;
      }
      console.log("MouseMove: ", this.currentIndex);
    },
    mouseDown(e: MouseEvent) {
      e.preventDefault();
      this.isMouseDown = 1;
      this.startSwipePosition = e.clientX;
      this.startSwipeIndex = this.currentIndex;
    },
    mouseUp(e: MouseEvent) {
      this.isMouseDown = 0;
      if (Math.abs(e.clientX - this.startSwipePosition) >= 150 && this.currentIndex < (this as any).swiperData.length - 1) {
        this.currentIndex = Math.round(this.currentIndex);
      } else {
        this.currentIndex = Math.round(this.startSwipeIndex);
      }

      console.log("MouseUp: ", this.currentIndex);
      this.startSwipePosition = 0;
    },
  },
  // computed: {
  //   currentSlide(index: number) {
  //     return {
  //       transform: `translate(-100% * ${index + 1})`,
  //     };
  //   },
  // },
});
</script>

<style scoped lang="scss">
.wrapper {
  display: flex;
  width: 300px;
  overflow: hidden;
  margin: 0 auto;
}
.swiping{
  transition: none;
}
</style>
