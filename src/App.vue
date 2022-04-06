<template>
  <div class="flex h-screen w-screen items-center justify-center bg-gray-900">
    <div class="flex w-screen">
      <Player
        v-for="video in videos"
        :key="video.v"
        v-bind="video"
        :play="play"
        @already="ready($event)"
      />
    </div>
    <Controller @playBtnClick="playBoth()" />
  </div>
</template>

<script>
import Player from "./components/Player.vue";
import Controller from "./components/Controller.vue";

// https://viewsync.net/watch?v=b6-2P8RgT0A&t=0&v=pHE29RXoor4&t=0&mode=solo&autoplay=false

// https://viewsync.net/watch?v=IEKPuHcKZ8Y&t=0&v=kpSCNSq7Egc&t=0&mode=solo&autoplay=false

export default {
  data() {
    return {
      videos: [
        {
          pid: 0,
          // v: "b6-2P8RgT0A",
          v: "",
          t: 0,
        },
        {
          pid: 1,
          // v: "pHE29RXoor4",
          v: "",
          t: 0,
        },
      ],
      play: false,
      count: 0,
      prev: {},
      aligned: false,
      timer: null,
      d: 0.07,
    };
  },
  beforeMount() {
    if (window.location.search) {
      const params = new URLSearchParams(window.location.search);
      this.videos[0].v = params.get("v1");
      this.videos[1].v = params.get("v2");
    }
  },
  mounted() {
    console.log(window.ytApiIsReady);
  },
  methods: {
    playBoth() {
      // if (this.count !== 2 && this.play) return;
      // this.play = !this.play;

      // if (this.play && !this.timer) {
      //   this.timerStart();
      // }

      // if (!this.play) {
      //   if (this.timer) {
      //     clearTimeout(this.timer);
      //   }
      // }
      this.play = !this.play;

      if (this.play) {
        this.videos[0].player.playVideo();
        this.timerStart();
        this.videos[1].player.playVideo();
      } else {
        if (this.timer) {
          clearTimeout(this.timer);
        }
      }
    },
    ready(e) {
      this.videos[e.pid].player = e.player;
    },
    update(e) {
      if (this.aligned) return;

      if (this.prev) {
        if (e.t < this.prev.t) {
          this.prev.player.seekTo(e.t);
        } else if (e.t > this.prev.t) {
          e.player.seekTo(this.prev.t);
        }
        // this.prev = {};
        // this.aligned = true;
      } else {
        this.prev = e;
      }
    },
    timerStart() {
      const v0t = this.videos[0].player.getCurrentTime();
      const v1t = this.videos[1].player.getCurrentTime();

      let a, b;

      if (Math.abs(v0t - v1t) > 0) {
        if (v0t > v1t) {
          a = this.videos[0].player;
          b = this.videos[1].player;
        } else {
          a = this.videos[1].player;
          b = this.videos[0].player;
        }

        if (a.getCurrentTime() - b.getCurrentTime() > this.d)
          a.seekTo(b.getCurrentTime());
      }

      // console.log(
      //   Math.abs(
      //     this.videos[1].player.getCurrentTime() -
      //       this.videos[0].player.getCurrentTime()
      //   )
      // );

      this.timer = setTimeout(this.timerStart, 500);
    },
  },
  components: {
    Player,
    Controller,
  },
};
</script>
