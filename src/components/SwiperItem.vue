<template>
  <div class="wrapper">
    {{item_data}}
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

</style>