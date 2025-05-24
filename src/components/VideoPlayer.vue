<template>
  <div class="video-player">
    <!-- Video -->
    <video
      ref="video"
      :src="videoSrc"
      @loadedmetadata="initTracking"
      @timeupdate="trackProgress"
      class="video-element"
    ></video>

    <!-- Controls -->
    <div class="controls">
      <button @click="rewind">⏪ 10s</button>
      <button @click="togglePlay">{{ isPlaying ? "⏸ Pause" : "▶️ Play" }}</button>
      <button @click="forward">⏩ 10s</button>
    </div>

    <!-- Progress Display -->
    <div class="progress-container">
      <div class="progress-bar">
        <div
          class="watched-progress"
          :style="{
            width: watchedPercentage + '%',
            backgroundColor: progressColor
          }"
        ></div>
        <div class="playhead" :style="{ left: playheadPercent + '%' }"></div>
      </div>
      <p class="percentage-display">
         Watch Progress: <strong>{{ watchedPercentage.toFixed(0) }}%</strong>
      </p>
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
      resolution: 1, // 1 second granularity
      watchedSegments: [],
    };
  },
  computed: {
    storageKey() {
      return `watched_${this.videoSrc}`;
    },
    watchedPercentage() {
      if (!this.videoDuration || this.watchedSegments.length === 0) return 0;
      const watchedCount = this.watchedSegments.filter(Boolean).length;
      return (watchedCount * this.resolution / this.watchedSegments.length) * 100;
    },
    playheadPercent() {
      // console.log(this.currentTime)
      return this.videoDuration > 0 ? (this.currentTime / this.videoDuration) * 100 : 0;
    },
    progressColor() {
      const p = this.watchedPercentage;
      if (p < 25) return "#f44336"; // red
      if (p < 50) return "#ff9800"; // orange
      if (p < 75) return "#ffeb3b"; // yellow
      return "#4caf50"; // green
    },
  },
  mounted(){
  
  },
  methods: {
    initTracking() {
      const video = this.$refs.video;
      this.videoDuration = video.duration; 
      // console.log("the video duration is",this.videoDuration  )
      const segmentCount = Math.ceil(video.duration / this.resolution);
      
      this.watchedSegments = saved
        ? JSON.parse(saved)
        : new Array(segmentCount).fill(false);
    },
    trackProgress() {
      const video = this.$refs.video;
      this.currentTime = video.currentTime;
      
      const index = Math.floor(video.currentTime / this.resolution);
      if (index >= 0 &&index < this.watchedSegments.length && !this.watchedSegments[index]
      ) {
        this.watchedSegments[index] = true; 
       
      }
    },
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

