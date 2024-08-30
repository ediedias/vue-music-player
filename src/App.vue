<script setup>
import { onMounted, onUnmounted, ref } from 'vue';

const songs = ref([
  { title: 'Master of Puppets Live', artist: 'Metallica', src: '/src/assets/metallica.mp3' },
  { title: 'Place for My Head', artist: 'Linkin Park', src: '/src/assets/linkin.mp3' },
  { title: 'My Name Is', artist: 'Eminem', src: '/src/assets/eminem.mp3' }
]);

const currentSongIndex = ref(0);
const currentSong = ref(songs.value[currentSongIndex.value]);
const isPlaying = ref(false);

const audio = ref(null);

const togglePlayPause = () => {
  if (!audio.value) return;

  if (isPlaying.value) {
    audio.value.pause();
  } else {
    audio.value.play();
  }
  isPlaying.value = !isPlaying.value;
}

const nextSong = () => {
  currentSongIndex.value = (currentSongIndex.value + 1) % songs.value.length;
  currentSong.value = songs.value[currentSongIndex.value];

  if (audio.value) {
    audio.value.load();

    if (isPlaying.value) {
      audio.value.play();
    }
  }

}

onMounted(() => {
  if (audio.value) {    
    audio.value.load();
  }
});

onUnmounted(() => {
  if (audio.value) {
    audio.value.pause();
  }
});

</script>

<template>
  <div class="flex flex-col items-center justify-center h-screen bg-gray-900 text-white">
    
    <h1 class="text-3xl font-bold mb-8">Music Player</h1>

    <div class="text-center">
      <h2 class="text-xl mb-2">{{ currentSong.title }}</h2>
      <p class="text-lg mb-4">{{ currentSong.artist }}</p>
    </div>

    <audio ref="audio" :src="currentSong.src"></audio>

    <div class="flex space-x-4">
      <button 
        @click="togglePlayPause"
        class="bg-blue-500 hover:bg-blue700 text-white font-bold py-2 px-4 rounded">
        {{ isPlaying ? 'Pause' : 'Play' }}
      </button>
      <button 
        @click="nextSong"
        class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded">Next</button>
    </div>
  </div>
</template>