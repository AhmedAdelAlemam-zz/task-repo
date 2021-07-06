<template>
  <div class="row">
    <div class="col-4 mb-5 pb-5">
      <!-- this filtering is not working yet -->
      <h5 class="mb-5">Upload Date</h5>
      <span
        class="d-block h6 pointer"
        :class="false ? 'font-weight-bold' : 'font-weight-light'"
      >
        Last hour
        <i v-if="false" class="fa fa-times pl-3"></i>
      </span>
      <span
        class="d-block h6 pointer"
        :class="false ? 'font-weight-bold' : 'font-weight-light'"
      >
        Today
        <i v-if="false" class="fa fa-times pl-3"></i>
      </span>
      <span
        class="d-block h6 pointer"
        :class="false ? 'font-weight-bold' : 'font-weight-light'"
      >
        This Week
        <i v-if="false" class="fa fa-times pl-3"></i>
      </span>
      <span
        class="d-block h6 pointer"
        :class="false ? 'font-weight-bold' : 'font-weight-light'"
      >
        This Month
        <i v-if="false" class="fa fa-times pl-3"></i>
      </span>
    </div>
    <div class="col-4 mb-5 pb-5">
      <!-- this filtering is working -->
      <h5 class="mb-5">Type</h5>
      <span
        class="d-block h6 pointer"
        :class="typePlaylist ? 'font-weight-bold' : 'font-weight-light'"
        v-on:click="fliterbyType"
      >
        Playlist
        <i v-if="typePlaylist" class="fa fa-times pl-3" v-on:click="reset"></i>
      </span>
      <span
        class="d-block h6 pointer"
        :class="typeVideo ? 'font-weight-bold' : 'font-weight-light'"
        v-on:click="fliterbyType"
      >
        Video
        <i v-if="typeVideo" class="fa fa-times pl-3" v-on:click="reset"></i>
      </span>
      <span
        class="d-block h6 pointer"
        :class="typeChannel ? 'font-weight-bold' : 'font-weight-light'"
        v-on:click="fliterbyType"
      >
        Channel
        <i v-if="typeChannel" class="fa fa-times pl-3" v-on:click="reset"></i>
      </span>
    </div>

    <!-- statistics -->
    <!-- this filtering is not working yet -->

    <div class="col-4 mb-5 pb-5">
      <h5 class="mb-5">Sort by</h5>
      <span
        class="d-block h6 pointer"
        :class="false ? 'font-weight-bold' : 'font-weight-light'"
      >
        Relevance
        <i v-if="false" class="fa fa-times pl-3"></i>
      </span>
      <span
        class="d-block h6 pointer"
        :class="false ? 'font-weight-bold' : 'font-weight-light'"
      >
        Upload Date
        <i v-if="false" class="fa fa-times pl-3"></i>
      </span>
      <span
        class="d-block h6 pointer"
        :class="false ? 'font-weight-bold' : 'font-weight-light'"
      >
        View Count
        <i v-if="false" class="fa fa-times pl-3"></i>
      </span>
      <span
        class="d-block h6 pointer"
        :class="false ? 'font-weight-bold' : 'font-weight-light'"
      >
        Rating
        <i v-if="false" class="fa fa-times pl-3"></i>
      </span>
    </div>
  </div>
</template>
<script>
import Axios from "axios";
export default {
  name: "FilterData",
  data() {
    return {
      typeVideo: false,
      typeChannel: false,
      typePlaylist: false,
      baseUrl: "https://youtube.googleapis.com/youtube/v3",
      part: this.$parent.$parent.api.part,
      type: this.$parent.$parent.api.type,
      order: this.$parent.$parent.api.order,
      maxResults: this.$parent.$parent.api.maxResults,
      q: this.$parent.$parent.api.q,
      key: this.$parent.$parent.api.key,
      requestedChannelId: "UCZTLU39XbfN04FBXVc0vD1g",
    };
  },
  methods: {
    //filtering data by type
    fliterbyType(e) {
      this.$parent.typeKeyword = e.target.innerText;
      this.$parent.keyword = e.target.innerText;
      if (e.target.innerText === "Video") {
        Axios.get(
          `${this.baseUrl}/videos?key=${this.key}&q=${this.q}&regionCode=CA&videoCategoryId=0&chart=mostPopular&part=${this.part}&order=${this.order}&maxResults=${this.maxResults}`
        ).then((res) => {
          this.$parent.$parent.videos = res.data.items;
          this.$parent.param = "watch?v=";
          this.typeVideo = true;
          this.typeChannel = false;
          this.typePlaylist = false;
          this.$parent.$parent.api.prevPageToken = res.data.prevPageToken;
          this.$parent.$parent.api.nextPageToken = res.data.nextPageToken;
        });
      } else if (e.target.innerText === "Channel") {
        Axios.get(
          `${this.baseUrl}/channels?id=${this.requestedChannelId}&key=${this.key}&q=${this.q}&part=${this.part}&type=${this.type}&order=${this.order}&maxResults=${this.maxResults}`
        ).then((res) => {
          this.$parent.$parent.videos = res.data.items;
          this.$parent.param = "channel/";
          this.typeVideo = false;
          this.typeChannel = true;
          this.typePlaylist = false;
          this.$parent.$parent.api.nextPageToken = "";
          this.$parent.$parent.api.prevPageToken = "";
        });
      } else if (e.target.innerText === "Playlist") {
        Axios.get(
          `${this.baseUrl}/playlists?key=${this.key}&channelId=${this.requestedChannelId}&q=${this.q}&part=${this.part}&order=${this.order}&maxResults=${this.maxResults}`
        ).then((res) => {
          this.$parent.$parent.videos = res.data.items;
          this.$parent.param = "playlist?list=";
          this.typeVideo = false;
          this.typeChannel = false;
          this.typePlaylist = true;
          this.$parent.$parent.api.nextPageToken = "";
          this.$parent.$parent.api.prevPageToken = "";
        });
      } else {
        this.reset();
      }
    },
    //reset data on click x
    reset() {
      Axios.get(
        `${this.baseUrl}/search?key=${this.key}&q=${this.q}&part=${this.part}&order=${this.order}&maxResults=${this.maxResults}`
      ).then((res) => {
        this.$parent.$parent.videos = res.data.items;
        this.$parent.param = "watch?v=";
        this.typeVideo = false;
        this.typeChannel = false;
        this.typePlaylist = false;
        this.$parent.$parent.api.prevPageToken = res.data.prevPageToken;
        this.$parent.$parent.api.nextPageToken = res.data.nextPageToken;
      });
    },
  },
};
</script>
<style scoped>
.pointer {
  cursor: pointer;
}
</style>
