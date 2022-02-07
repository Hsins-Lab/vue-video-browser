<template>
  <div>
    <SearchBar v-on:termChange="onTermChange"></SearchBar>
    <div class="row">
      <VideoDetail :video="selectedVideo" />
      <VideoList @videoSelect="onVideoSelect" :videos="videos"></VideoList>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import SearchBar from './components/SearchBar';
import VideoList from './components/VideoList';
import VideoDetail from './components/VideoDetail';

const API_KEY = 'AIzaSyB0ToJFPdt3Dp2stb8ynXqHkNe9U0ivB0c';

export default {
  name: 'App',
  components: {
    SearchBar,
    VideoList,
    VideoDetail,
  },
  data() {
    return { videos: [], selectedVideo: null };
  },
  methods: {
    onVideoSelect(video) {
      this.selectedVideo = video;
    },
    onTermChange(searchTerm) {
      axios
        .get('https://www.googleapis.com/youtube/v3/search', {
          params: {
            key: API_KEY,
            type: 'video',
            part: 'snippet',
            q: searchTerm,
          },
        })
        .then((response) => {
          this.videos = response.data.items;
        });
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
