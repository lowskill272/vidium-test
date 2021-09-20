<template>
  <div class="wrapper">
        <Swiper />
    <div class="swiper-wrapper">
      <div class="episode-wrapper"
           v-for="episode in episodes"
           :key="episode.id">
        <div
          class="episode" :class="this.episodeState(episode)"
        >
          <div class="border" v-show="this.episodeState(episode) === 'live'"></div>
          <div class="left">
            <div class="episode-time">
              <div class="icon"></div>
              <div class="value">{{ this.formatTime(episode) }}</div>
            </div>
            <div
              class="episode-live"
              v-show="this.episodeState(episode) === 'live'"
            >
              Эфир
            </div>
          </div>
          <div class="right">
            <div class="episode-title">{{ episode.lines.title }}</div>
            <div class="episode-description">{{ episode.lines.description }}</div>
            <div class="episode-speakers">{{ this.formatSpeakers(episode) }}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <button class="test" @click="this.test()">Загрузить</button>
</template>

<script lang="ts">
import Swiper from "@/components/Swiper.vue";
import { Options, Vue } from "vue-class-component";

type Episode = {
  id: number;
  plannedTime: string;
  duration: string;
  startTime: string;
  lines: {
    title: string;
    description: string;
    title_short: string;
    speakers: [
      {
        name: string;
        title: string;
      }
    ];
  };
  face: null | string;
  type: string;
  data: any;
  speakers: [
    {
      name: string;
    }
  ];
  stopTime: string;
};

@Options({
  components: {
    Swiper,
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

    return await response.json();
  }

  test() {
    console.log(this.episodes);
  }

  formatSpeakers(episode: Episode) {
    if (typeof episode.speakers == "undefined") {
      return "";
    }

    return episode.speakers
      .map((elem) => {
        return elem.name;
      })
      .join(", ");
  }

  formatTime(episode: Episode) {
    const episodeDate = new Date(episode.plannedTime);

    const hours = episodeDate.getHours();
    const minutes =
      episodeDate.getMinutes() < 10
        ? `${episodeDate.getMinutes()}0`
        : episodeDate.getMinutes();

    return hours + ":" + minutes;
  }

  episodeState(episode: Episode) {
    const episodeDateStart = new Date(episode.startTime);
    const episodeDateEnd = new Date(episode.stopTime);
    const curDate = new Date();

    return "live";

    // if (episodeDateStart <= curDate && episodeDateEnd >= curDate) {
    //   return "live";
    // } else if (episodeDateStart > curDate) {
    //   return "next";
    // } else if (episodeDateEnd < curDate) {
    //   return "previous";
    // }
  }

  mounted(): void {
    this.getEpisodes().then((episodes) => (this.episodes = episodes));
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
$base-color: #456eff;
$shine-color: #f04d4d;
$animation-duration: 1.6s;

$episode-width: 374px;
$episode-min-height: 86px;

@mixin background-gradient {
  background-image: linear-gradient(90deg, $shine-color 0%, $base-color 100%);
  background-size: 600px;
}

@mixin prevDefault {
  color: #bebebe;
  border: none;
}

@mixin prevHover {
  color: #9f9f9f;
  border: none;
}

@mixin nextDefault {
  color: #9f9f9f;
  border: 1px solid #dfdddd;
}

@mixin nextHover {
  color: #404040;
  border: 1px solid #dfdddd;
}

@mixin nextActive {
  color: #404040;
  border: 2px solid #d63d3d;
}

@mixin liveDefault {
  color: #9f9f9f;
  position: relative;

  .border {
    border-radius: 1rem;
    z-index: -1;
    width: calc(100% + 2px);
    height: calc(100% + 2px);
    position: absolute;
    left: -1px;
    top: -1px;
    background: linear-gradient(90deg, #456eff 0%, #f04d4d 100%);
    @include background-gradient;
    animation: shine-lines $animation-duration infinite linear;
  }
}

@mixin liveHover {
  color: #404040;
  position: relative;

  .border {
    border-radius: 1rem;
    width: calc(100% + 2px);
    height: calc(100% + 2px);
    position: absolute;
    left: -1px;
    top: -1px;
    background: linear-gradient(90deg, #456eff 0%, #f04d4d 100%);
    @include background-gradient;
    animation: shine-lines $animation-duration infinite linear;
  }
}

@mixin liveActive {
  color: #404040;
  position: relative;

  .border {
    border-radius: 1rem;
    width: calc(100% + 4px);
    height: calc(100% + 4px);
    position: absolute;
    left: -2px;
    top: -2px;
    background: linear-gradient(90deg, #456eff 0%, #f04d4d 100%);
    @include background-gradient;
    animation: shine-lines $animation-duration infinite linear;
  }
}

@keyframes shine-lines {
  0% {
    background-position: -100px;
  }
  40%,
  100% {
    background-position: 376px;
  }
}


.swiper-wrapper {
  display: flex;
  align-items: flex-start;
  max-width: 100%;
  //overflow: hidden;
  margin: 0 auto;
  padding: 0 100px;
  font-family: "Montserrat", sans-serif;

  .episode {
    display: flex;
    width: $episode-width;
    min-height: $episode-min-height;
    background: #ffffff;
    box-sizing: border-box;
    border-radius: 10px;

    &-wrapper{
      margin: 0 0.5rem;
      padding: 1px;
      z-index: 1;

      &:hover {
        .episode-description {
          max-height: 999px;
          padding: 1rem 0;
        }
      }
    }



    &.previous {
      @include prevDefault;
    }

    &.next {
      @include nextDefault;
    }

    &.live {
      @include liveDefault;
    }

    &.previous:hover {
      @include prevHover;
    }

    &.next:hover {
      @include nextHover;
    }

    &.live:hover {
      @include liveHover;
    }

    .left {
      width: 20%;
      padding: 1rem;
    }

    .right {
    }

    &-title {
      padding: 0.7rem 1rem 0.4rem 0;
      line-height: 24px;
      font-size: 15px;
    }

    &-description {
      max-height: 0;
      overflow: hidden;
      transition: max-height 0.5s ease, padding 0.25s ease;
      line-height: 22px;
      font-size: 13px;
    }

    &-description,
    &-title {
      font-style: normal;
      font-weight: 500;
      letter-spacing: 0em;
      text-align: left;
    }
    &-speakers {
      font-size: 14px;
      font-style: normal;
      font-weight: 500;
      line-height: 22px;
      letter-spacing: 0em;
      text-align: left;
      padding-bottom: 0.7rem;
      color: #9f9f9f;
    }

    &-time {
      display: flex;
      align-items: center;
      margin-bottom: 1rem;

      .icon {
        background-color: #bebebe;
        mask-image: url(../assets/clock.svg);
        width: 12px;
        height: 12px;
        margin-right: 0.2rem;
      }
      .value {
        font-size: 15px;
        font-style: normal;
        font-weight: 500;
        line-height: 18px;
        letter-spacing: -0.07em;
        text-align: left;
        color: #bebebe;
      }
    }

    &-live {
      font-size: 14px;
      font-style: normal;
      font-weight: 500;
      line-height: 17px;
      letter-spacing: 0;
      text-align: left;
      color: #9F9F9F;
      padding-left: 6px;
      position: relative;

      &:before {
        content: "";
        width: 3px;
        height: 3px;
        background-color: red;
        position: absolute;
        left: 0;
        top: calc(50% - 3px);
      }
    }
  }
}
</style>
