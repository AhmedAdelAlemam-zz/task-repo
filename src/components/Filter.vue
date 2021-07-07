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
        @click="fliterByType"
      >
        Playlist
        <i v-if="typePlaylist" class="fa fa-times pl-3" @click="reset()"></i>
      </span>
      <span
        class="d-block h6 pointer"
        :class="typeVideo ? 'font-weight-bold' : 'font-weight-light'"
        @click="fliterByType"
      >
        Video
        <i v-if="typeVideo" class="fa fa-times pl-3" @click="reset()"></i>
      </span>
      <span
        class="d-block h6 pointer"
        :class="typeChannel ? 'font-weight-bold' : 'font-weight-light'"
        @click="fliterByType"
      >
        Channel
        <i v-if="typeChannel" class="fa fa-times pl-3" @click="reset()"></i>
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
export default {
  name: "FilterData",
  props: ["fliterByType", "typeVideo", "typeChannel", "typePlaylist"],
};
</script>
<style scoped>
.pointer {
  cursor: pointer;
}
</style>
