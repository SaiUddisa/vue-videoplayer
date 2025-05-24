<template>
  <div class="video-player">
    <!-- Video -->
    <video
      ref="video"
      :src="videoSrc"
      class="video-element"
    ></video>

    <!-- Controls -->
    <div class="controls">
      <button @click="rewind">⏪ 10s</button>
      <button @click="togglePlay">{{ isPlaying ? "⏸ Pause" : "▶️ Play" }}</button>
      <button @click="forward">⏩ 10s</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "VideoPlayer",
  props: {
    videoSrc: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      isPlaying: false,
      currentTime: 0,
      videoDuration: 0,
      resolution: 1, 
      watchedSegments: [],
    };
  },

  mounted(){
  
  },
  methods: {
  
   
    togglePlay() {
      const video = this.$refs.video;
      if (video.paused) {
        video.play();
        this.isPlaying = true;
      } else {
        video.pause();
        this.isPlaying = false;
      }
    },
    forward() {
      const video = this.$refs.video;
      video.currentTime = Math.min(video.duration, video.currentTime + 10);
    },
    rewind() {
      const video = this.$refs.video;
      video.currentTime = Math.max(0, video.currentTime - 10);
    },
  },
};
</script>


