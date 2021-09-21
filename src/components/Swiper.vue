<template>
  <div class="wrapper">
    <swiper-item
      v-for="(item, index) in swiperData"
      :key="index"
      :item_data="item"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import SwiperItem from "@/components/SwiperItem.vue";

export default defineComponent({
  name: 'Swiper',
  components: {
    SwiperItem
  },
  props: {
    swiperData: [],

  }
})
</script>

<style scoped lang="scss">
.wrapper{
  display: flex;
}
</style>
