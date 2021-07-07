<template>
  <div
    class="col-6 col-lg-5"
    v-if="isPlaylistItems"
  >
    <h4 class="text-info mb-3">Playlist items</h4>
    <div :key="item.id" v-for="item in playlistItems">
      <a
        :href="
          `https://www.youtube.com/watch?v=${item.snippet.resourceId.videoId}`
        "
        class="card-link text-decoration-none"
        target="_blank"
      >
        <div class="row mb-3 border p-3">
          <div class="col-12 col-lg-3">
            <img
              class="img-fluid"
              :src="item.snippet.thumbnails.medium.url"
              alt="YouTube thumbnail"
            />
          </div>
          <div class="col-12 col-lg-9">
            <h6 class="card-title text-dark">
              {{ item.snippet.title }}
            </h6>
            <h6 class="card-subtitle mb-2 text-muted">
              {{ item.snippet.channelTitle }} |
              {{ item.snippet.publishedAt | formatDate }}
            </h6>
            <p class="text-muted" v-if="item.snippet.description">
              {{ item.snippet.description.slice(0, 100) }}...
            </p>
          </div>
        </div>
      </a>
    </div>
  </div>
</template>

<script>
import Axios from "axios";
export default {
  name: "PlaylistItem",
  data() {
    return {
      playlistItems: [],
      part: this.$parent.$parent.$parent.api.part,
      type: this.$parent.$parent.$parent.api.type,
      order: this.$parent.$parent.$parent.api.order,
      maxResults: this.$parent.$parent.$parent.api.maxResults,
      q: this.$parent.$parent.$parent.api.q,
      key: this.$parent.$parent.$parent.api.key,
    };
  },
  props: ["isPlaylistItems", "id"],
  methods: {
    // getting playlist items
    getPlaylistItems() {
      Axios.get(
        `https://youtube.googleapis.com/youtube/v3/playlistItems?key=${this.key}&playlistId=${this.id}&${this.q}&part=${this.part}&order=${this.order}&maxResults=${this.maxResults}`
      ).then((res) => {
        this.playlistItems = res.data.items;
      });
    },
  },
  mounted() {
    // calling playlist items method if filtering type is playlist
    this.$parent.$parent.keyword === "Playlist" &&
    this.$parent.$parent.typeKeyword === "Playlist"
      ? this.getPlaylistItems()
      : null;
  },
};
</script>
