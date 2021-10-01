<template>
  <div class="wrapper">
    <slot></slot>
  </div>
</template>

<script lang="ts">

import { defineComponent } from 'vue'

export default defineComponent({
  name: 'SwiperItem',
  props: {
    item_data: []
  }
})
</script>

<style scoped>
.wrapper{
  /*min-width: 300px;*/
  /*display: flex;*/
  /*justify-content: center;*/
  /*align-items: center;*/
  /*border: 1px solid #e5e5e5;*/
  /*box-sizing: border-box;*/
  /*border-radius: 10px;*/
  /*transition: transform 0.5s ease;*/
  /*cursor: pointer;*/
  opacity: 0.5;
}
</style>