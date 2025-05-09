<template>
    <div class="now-playing">
      <div class="controls">
        <button @click="togglePlayback" :aria-label="isPlaying ? 'Pause' : 'Play'">
          <span v-if="isPlaying">⏸</span>
          <span v-else>▶</span>
        </button>
        <input type="range" min="0" max="1" step="0.01" v-model="volume" @input="updateVolume" />
      </div>
      <div class="metadata">
        <!-- <div class="artwork">
          <img :src="artworkUrl" alt="Station Artwork" />
        </div> -->
        <div class="text">
          <strong>{{ streamTitle }}</strong><br />
          <small>{{ streamDesc }}</small>
        </div>
      </div>
      <audio ref="player" :src="streamUrl" preload="none" />
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  
  const streamUrl = 'http://kteq-streamer.sdsmt.edu:8000/kteq_high';
  const streamTitle = ref('Stream KTEQ-FM 91.3');
//   const streamDesc = ref('Student-Run Radio from South Dakota Mines');
//   const artworkUrl = ref('/assets/logo-placeholder.png');
  
  const player = ref(null);
  const isPlaying = ref(false);
  const volume = ref(0.8);
  
  function togglePlayback() {
    if (!player.value) return;
    if (isPlaying.value) {
      player.value.pause();
    } else {
      player.value.play();
    }
    isPlaying.value = !isPlaying.value;
  }
  
  function updateVolume() {
    if (player.value) {
      player.value.volume = volume.value;
    }
  }
  
  onMounted(() => {
    updateVolume();
  });
  </script>
  
  <style scoped>
  .now-playing {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 1rem;
    background: #1f1f1f;
    color: #f0f0f0;
    padding: 0.5rem 1rem;
  }
  
  .controls {
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }
  
  .controls button {
    background: none;
    border: none;
    font-size: 1.5rem;
    color: #fceb00;
    cursor: pointer;
  }
  
  .metadata {
    display: flex;
    align-items: center;
    gap: 0.75rem;
  }
  
  .artwork img {
    width: 48px;
    height: 48px;
    object-fit: cover;
    border-radius: 6px;
  }
  </style>
  