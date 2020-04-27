<template>
  <div class="container">
    <SearchBar @termChange="onTermChange"></SearchBar>
    <div class="row">
      <VideoDetail :video="selectedVideo" />
      <VideoList @videoSelect="onVideoSelect" :videos="videos"></VideoList>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import SearchBar from './components/SearchBar.vue';
import { config } from './config';
import VideoList from './components/VideoList.vue';
import VideoDetail from './components/VideoDetail.vue';

export default {
  name: 'App',
  components: {
    SearchBar,
    VideoList,
    VideoDetail,
  },
  data() {
    return {
      videos: [],
      selectedVideo: null,
    };
  },
  methods: {
    onTermChange(searchTerm) {
      axios
        .get('https://www.googleapis.com/youtube/v3/search', {
          params: {
            key: config.YOUTUBE_API,
            type: 'video',
            part: 'snippet',
            q: searchTerm,
          },
        })
        .then((res) => {
          this.videos = res.data.items;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    onVideoSelect(video) {
      this.selectedVideo = video;
    },
  },
};
</script>

<style></style>
