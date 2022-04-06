<template>
  <div
    :id="`player${pid ?? 1}`"
    class="!aspect-video w-1/2 flex-1 overflow-hidden"
  ></div>
</template>

<script>
export default {
  props: ["pid", "v", "t", "play"],
  data() {
    return {
      player: null,
      isReady: false,
      timer: null,
    };
  },
  mounted() {
    if (window.ytApiIsReady) {
      this.playerInit();
    }
  },
  watch: {
    play(e) {
      if (e) {
        this.player.playVideo();
        // this.timerStart();
      } else {
        this.player.stopVideo();
        // clearTimeout(this.timer);
      }
    },
  },
  methods: {
    playerInit() {
      this.player = new YT.Player(`player${this.pid ?? 1}`, {
        videoId: this.v ?? "Xa0Q0J5tOP0",
        width: window.width / 2,
        height: (window.width * 16) / 18,
        events: {
          onReady: this.onPlayerReady,
          onStateChange: this.onPlayerStateChange,
        },
      });
    },
    onPlayerReady(evt) {
      this.$emit("already", {
        pid: this.pid,
        player: this.player,
      });
    },
    onPlayerStateChange(evt) {
      // console.log("Player state changed", evt);
    },
    timerStart() {
      this.$emit("updated", {
        t: this.player.getCurrentTime(),
        pid: this.pid,
        player: this.player,
      });

      // console.log(`video${this.pid} emitted`);

      this.timer = setTimeout(this.timerStart, 50);
    },
  },
};
</script>

<style></style>
