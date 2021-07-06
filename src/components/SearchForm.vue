<template>
  <div class="row">
    <div class="col-4 col-lg-1">
      <img class="img-fluid" src="../../public/img/youtube-logo.png" alt="" />
    </div>
    <div class="col-8 col-lg-9 mx-auto my-0">
      <form class="mb-5">
        <div class="input-group">
          <input
            v-model="searchString"
            @keydown.13.prevent="parseSearchString"
            type="text"
            class="form-control"
            placeholder="Search ..."
          />
          <div class="input-group-append">
            <button
              @click="parseSearchString"
              class="btn btn-outline-secondary"
              type="button"
            >
              <i class="fas fa-search"></i>
            </button>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: "SearchForm",
  data() {
    return {
      searchString: "",
    };
  },
  methods: {
    parseSearchString() {
      // Trim search string
      const trimmedSearchString = this.searchString.trim();

      if (trimmedSearchString !== "") {
        // Split search string
        const searchParams = trimmedSearchString.split(/\s+/);
        // Emit event
        this.$emit("search", searchParams);
      }
    },
  },
};
</script>

<style scoped>
input,
button {
  box-shadow: none !important;
}

.form-control {
  border-color: #6c757d;
}
</style>
