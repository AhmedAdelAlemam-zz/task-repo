<template>
  <div id="app">
    <div class="container-fluid mt-5">
      <div class="row">
        <div class="col-11 col-lg-8 mx-auto my-0 ">
          <SearchForm v-on:search="search" />
          <SearchResults
            v-if="videos.length > 0"
            :videos="videos"
            :reformattedSearchString="reformattedSearchString"
            :totalResults="totalResults"
            v-model="videos"
          />
          <Pagination
            v-if="
              videos.length > 0 &&
                api.nextPageToken !== '' &&
                api.prevPageToken !== ''
            "
            :prevPageToken="api.prevPageToken"
            :nextPageToken="api.nextPageToken"
            v-on:prev-page="prevPage"
            v-on:next-page="nextPage"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import SearchForm from "./components/SearchForm";
import SearchResults from "./components/SearchResults";
import Pagination from "./components/Pagination";
import Axios from "axios";
export default {
  name: "app",
  components: {
    SearchForm,
    SearchResults,
    Pagination,
  },
  data() {
    return {
      videos: [],
      reformattedSearchString: "",
      totalResults: Number,
      api: {
        searchUrl: "https://youtube.googleapis.com/youtube/v3/search",
        part: "snippet",
        type: "video",
        key: "AIzaSyDbn099qsVEAR3E_MZtK6Pgj_I0x3Rdqf4",
        order: "viewCount",
        maxResults: 15,
        q: "",
        prevPageToken: "",
        nextPageToken: "",
      },
    };
  },
  methods: {
    // getting search data after enter keyword
    search(searchParams) {
      this.reformattedSearchString = searchParams.join(" ");
      this.api.q = searchParams.join("+");

      const apiUrl = `${this.api.searchUrl}?key=${this.api.key}&part=${this.api.part}&type=${this.api.type}&order=${this.api.order}&q=${this.api.q}&maxResults=${this.api.maxResults}`;
      this.getData(apiUrl);
    },

    // pagination
    prevPage() {
      const apiUrl = `${this.api.searchUrl}?key=${this.api.key}&part=${this.api.part}&type=${this.api.type}&order=${this.api.order}&q=${this.api.q}&maxResults=${this.api.maxResults}&pageToken=${this.api.prevPageToken}`;
      this.getData(apiUrl);
    },

    nextPage() {
      const apiUrl = `${this.api.searchUrl}?key=${this.api.key}&part=${this.api.part}&type=${this.api.type}&order=${this.api.order}&q=${this.api.q}&maxResults=${this.api.maxResults}&pageToken=${this.api.nextPageToken}`;
      this.getData(apiUrl);
    },

    // get data on search
    getData(url) {
      Axios(url)
        .then((res) => {
          this.videos = res.data.items;
          this.api.prevPageToken = res.data.prevPageToken;
          this.api.nextPageToken = res.data.nextPageToken;
          this.totalResults = res.data.pageInfo.totalResults
            .toString()
            .replace(/\B(?<!\.\d*)(?=(\d{3})+(?!\d))/g, ",");
        })
        .catch((error) => console.log(error));
    },
  },
};
</script>
