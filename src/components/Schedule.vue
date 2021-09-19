<template>
  <div class="wrapper">
    <Swiper/>
  </div>
</template>

<script lang="ts">

import Swiper from "@/components/Swiper.vue";
import { Options, Vue } from "vue-class-component";

type Episode = {
  id: number,
  plannedTime: string,
  duration: string,
  startTime: string,
  lines: {
    title: string,
    description: string,
    title_short: string,
    speakers: [
      {
        name: string,
        title: string
      }
    ]
  },
  face: null | string,
  type: string,
  data: any,
  speakers: [
    {
      name: string
    }
  ],
  stopTime: string
};

@Options({
  components: {
    Schedule,
  },
})
export default class Schedule extends Vue {
  // data
  episodes: Episode[] = [];

  // methods
  async getEpisodes(): Promise<Episode[]> {
    let response = await fetch(
      "https://api.d-02.niceplayer.ru/v1/events/20210619-scheduletest/timetable/episodes"
    );

    let result = await response.text();

    let epis: Episode[] = await JSON.parse(result);

    console.log(typeof epis);

    return epis;
  }

  mounted(): void {
    this.getEpisodes().then(episodes => this.episodes = episodes);
    console.log(this.episodes);
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

</style>
