<template>
  <div class="wrapper">
    <SwiperItem />
  </div>
</template>

<script lang="ts">
import SwiperItem from "@/components/SwiperItem.vue";
import { Options, Vue } from "vue-class-component";

@Options({
  components: {
    SwiperItem,
  },
})
export default class Schedule extends Vue {
  // data
}
</script>

<style scoped></style>
