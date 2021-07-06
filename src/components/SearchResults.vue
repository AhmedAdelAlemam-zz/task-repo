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
        <FilterData ref="filter" v-if="showFilter" />
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
        />
      </div>
    </div>
  </div>
</template>

<script>
import VideoListItem from "./VideoListItem";
import FilterData from "./Filter";
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
      param: "watch?v=",
      id: "",
      typeKeyword: "",
      keyword: "",
    };
  },
  props: ["videos", "reformattedSearchString", "totalResults"],
  methods: {
    showFilterOnClick() {
      this.showFilter = !this.showFilter;
    },
  },
};
</script>

<style scoped>
button:focus {
  box-shadow: none !important;
}
</style>
