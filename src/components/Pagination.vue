<template>
  <div class="container">
    <nav>
      <ul class="pagination justify-content-end">
        <li
          class="page-item"
          :class="{ disabled: prevPageToken === undefined }"
        ></li>
        <li
          class="page-item"
          :class="{ disabled: nextPageToken === undefined }"
        >
          <a v-on:click="nextPage" class="page-link" href="#">Next</a>
        </li>
        <li
          class="page-item"
          :class="{ disabled: prevPageToken === undefined }"
        >
          <a v-on:click="prevPage" class="page-link" href="#">Previous</a>
        </li>
      </ul>
    </nav>
  </div>
</template>

<script>
export default {
  name: "Pagination",
  props: ["prevPageToken", "nextPageToken"],
  methods: {
    prevPage() {
      this.$emit("prev-page");
    },
    nextPage() {
      this.$emit("next-page");
    },
  },
};
</script>

<style scoped>
.page-link {
  box-shadow: none !important;
}
</style>
