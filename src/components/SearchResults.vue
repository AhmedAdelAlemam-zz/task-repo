<template>
  <div class="mb-3">
    <div class="row mb-3">
      <div class="col-10">
        <h5>
          About {{ totalResults }} Results for "{{ reformattedSearchString }}"
        </h5>
      </div>
      <div class="col-2">
        <button class="btn" @click="showFilterOnClick">
          <i class="fa fa-filter h4"></i>
          <span class="h4">Filter</span>
        </button>
      </div>
      <div class="col-12">
        <FilterData
          ref="filter"
          :fliterByType="fliterByType"
          :typeVideo="typeVideo"
          :typeChannel="typeChannel"
          :typePlaylist="typePlaylist"
          v-if="showFilter"
        />
      </div>
    </div>

    <div>
      <div
        :videos="videos"
        class="mb-4"
        :key="item.id.videoId"
        v-for="item in videos"
      >
        <VideoListItem
          ref="videolistitem"
          :id="
            typeKeyword && keyword !== '' && typeKeyword === keyword
              ? item.id
              : item.id.videoId
          "
          :param="param"
          :item="item"
          :isPlaylistItems="isPlaylistItems"
          :isChannelSections="isChannelSections"
        />
      </div>
    </div>
  </div>
</template>

<script>
import VideoListItem from "./VideoListItem";
import FilterData from "./Filter";
import Axios from "axios";
export default {
  name: "SearchResults",
  components: {
    VideoListItem,
    FilterData,
  },
  data() {
    return {
      title: "Search Results",
      showFilter: false,
      typeVideo: false,
      typeChannel: false,
      typePlaylist: false,
      isPlaylistItems: false,
      isChannelSections: false,
      param: "watch?v=",
      id: "",
      typeKeyword: "",
      keyword: "",
      baseUrl: "https://youtube.googleapis.com/youtube/v3",
      part: this.$parent.api.part,
      type: this.$parent.api.type,
      order: this.$parent.api.order,
      maxResults: this.$parent.api.maxResults,
      q: this.$parent.api.q,
      key: this.$parent.api.key,
      requestedChannelId: "UCSJbGtTlrDami-tDGPUV9-w",
    };
  },
  props: ["videos", "reformattedSearchString", "totalResults"],
  methods: {
    showFilterOnClick() {
      this.showFilter = !this.showFilter;
    },
    //filtering data by type
    fliterByType(e) {
      this.typeKeyword = e.target.innerText;
      this.keyword = e.target.innerText;
      if (e.target.innerText === "Video") {
        Axios.get(
          `${this.baseUrl}/videos?key=${this.key}&q=${this.q}&regionCode=CA&videoCategoryId=0&chart=mostPopular&part=${this.part}&order=${this.order}&maxResults=${this.maxResults}`
        ).then((res) => {
          this.$parent.videos = res.data.items;
          this.param = "watch?v=";
          this.typeVideo = true;
          this.typeChannel = false;
          this.typePlaylist = false;
          this.isPlaylistItems = false;
          this.isChannelSections = false;
          this.$parent.api.prevPageToken = res.data.prevPageToken;
          this.$parent.api.nextPageToken = res.data.nextPageToken;
        });
      } else if (e.target.innerText === "Channel") {
        Axios.get(
          `${this.baseUrl}/channels?id=${this.requestedChannelId}&key=${this.key}&q=${this.q}&part=${this.part}&type=${this.type}&order=${this.order}&maxResults=${this.maxResults}`
        )
          .then((res) => {
            this.$parent.videos = res.data.items;
            this.param = "channel/";
            this.typeVideo = false;
            this.typeChannel = true;
            this.typePlaylist = false;
            this.isPlaylistItems = false;
            this.isChannelSections = true;
            this.$parent.api.nextPageToken = "";
            this.$parent.api.prevPageToken = "";
          })
          .then(() => {
            // getting channel sections
            Axios.get(
              `https://youtube.googleapis.com/youtube/v3/channelSections?key=${this.key}&channelId=${this.requestedChannelId}&part=${this.part}&type=${this.type}&order=${this.order}&q=${this.q}&maxResults=${this.maxResults}`
            ).then((result) => {
              console.log(result);
            });
          });
      } else if (e.target.innerText === "Playlist") {
        Axios.get(
          `${this.baseUrl}/playlists?key=${this.key}&channelId=${this.requestedChannelId}&q=${this.q}&part=${this.part}&order=${this.order}&maxResults=${this.maxResults}`
        ).then((res) => {
          this.$parent.videos = res.data.items;
          this.param = "playlist?list=";
          this.typeVideo = false;
          this.typeChannel = false;
          this.typePlaylist = true;
          this.isPlaylistItems = true;
          this.isChannelSections = false;
          this.$parent.api.nextPageToken = "";
          this.$parent.api.prevPageToken = "";
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
        this.$parent.videos = res.data.items;
        this.param = "watch?v=";
        this.typeVideo = false;
        this.typeChannel = false;
        this.typePlaylist = false;
        this.isChannelSections = false;
        this.$parent.api.prevPageToken = res.data.prevPageToken;
        this.$parent.api.nextPageToken = res.data.nextPageToken;
      });
    },
  },
};
</script>

<style scoped>
button:focus {
  box-shadow: none !important;
}
</style>
