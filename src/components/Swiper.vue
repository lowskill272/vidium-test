<template>
  <div class="wrapper">
    <div
      class="track"
      :style="{ transform: 'translate(-' + currentOffset + 'px)' }"
      :class="{ swiping: isMouseDown }"
      @mousemove="mouseMove($event)"
      @mouseup="mouseUp($event)"
      @mousedown="mouseDown($event)"
    >
      <slot></slot>
    </div>
    <button class="prev" @click="prevSlide">Prev</button>
    <button class="next" @click="nextSlide">Next</button>
  </div>
</template>

<script lang="ts">
import { defineComponent, VNode, h } from "vue";
import { extend } from "swiper/angular/angular/src/utils/utils";

export default defineComponent({
  name: "Swiper",
  components: {},
  // render(): VNode {
  //   return h(
  //     'div',
  //     {
  //         class: 'wrapper'
  //       },
  //       h(
  //         'div',
  //         {
  //           class: 'track',
  //         },
  //         (this as any).$slots.default()
  //       )
  //   );
  // },
  props: {
    slidesPerView: {
      type: Number,
      default: 1,
    },
    spaceBetween: {
      type: Number,
      default: 50,
    },
  },
  data() {
    return {
      currentIndex: 0,
      startSwipePosition: 0,
      startSwipeIndex: 0,
      isMouseDown: 0,
      slideWidth: 0,
      slides: [] as HTMLElement[]
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
      if (
        this.currentIndex <
        (this.$el.childNodes[0].childNodes.length) /
          (this as any).slidesPerView - 1
      ) {
        this.currentIndex++;
      }
      console.log(this.currentIndex);
    },
    mouseMove(e: MouseEvent) {
      console.log(
        this.startSwipeIndex,
        this.startSwipePosition,
        (this as any).slideWidth * (this as any).slidesPerView
      );
      if (this.isMouseDown) {
        this.currentIndex =
          this.startSwipeIndex +
          (this.startSwipePosition - e.clientX) /
            ((this as any).slideWidth * (this as any).slidesPerView);
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
      if (
        Math.abs(e.clientX - this.startSwipePosition) >=
          (this as any).slideWidth / 2 &&
        Math.round(this.currentIndex) <
          (this.$el.childNodes[0].childNodes.length) /
            (this as any).slidesPerView - 1
      ) {
        this.currentIndex = Math.round(this.currentIndex);
      } else {
        this.currentIndex = Math.round(this.startSwipeIndex);
      }

      this.slides.forEach((slide: HTMLElement, index) => {
        if (
          index < this.slidesPerView * (this.currentIndex + 1) &&
          index >= this.slidesPerView * this.currentIndex
        ) {
          slide.classList.add('active');
        } else {
          slide.classList.remove('active');
        }
      });

      console.log("MouseUp: ", this.currentIndex);
      this.startSwipePosition = 0;
    },
  },
  mounted() {
    console.log( this.$el.childNodes[0].clientWidth, this.$el.clientWidth);
    this.slideWidth =
      this.$el.childNodes[0].clientWidth / this.slidesPerView - this.spaceBetween;
    setTimeout(() => {
      this.$el.childNodes[0].children.forEach((slide: HTMLElement, index: number) => {
        this.slides.push(slide);

        slide.style.minWidth = this.slideWidth + "px";
        slide.style.margin = "0 " + this.spaceBetween / 2 + "px";

        if (
          index < this.slidesPerView * (this.currentIndex + 1) &&
          index >= this.slidesPerView * this.currentIndex
        ) {
          slide.classList.add('active');
        } else {
          slide.classList.remove('active');
        }
      });
    }, 500);
  },
  computed: {
    currentOffset() {
      console.log(this.slideWidth, this.slidesPerView, this.currentIndex);
      return (
        (this.slideWidth + this.spaceBetween) *
        this.slidesPerView *
        this.currentIndex
      );
    },
  },
});
</script>

<style lang="scss">
.wrapper {
  display: flex;
  //width: 300px;
  margin: 0 auto;
  position: relative;
  width: 100%;
  height: 100%;

  .prev, next{
    position: absolute;
    top: 50%;
  }

  .prev{
    left: 5%;
  }

  .next{
    right: 5%;
  }
}
.track {
  position: absolute;
  display: flex;
  transition: transform 0.5s ease;
  width: 80%;
  margin-left: 10%;
}
.buttons {
  padding-top: 300px;
}
.swiping {
  transition: none;
}
.active {
  opacity: 1 !important;
}
</style>
