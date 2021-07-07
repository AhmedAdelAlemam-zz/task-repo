<template>
  <div>
    <!-- video/playlist items -->
    <a
      :href="`https://www.youtube.com/${param}${id}`"
      class="card-link text-decoration-none"
      target="_blank"
    >
      <div
        :class="
          isPlaylistItems || isChannelSections ? 'row border-bottom' : 'row'
        "
      >
        <div
          :class="
            isPlaylistItems || isChannelSections ? 'col-6 col-lg-7' : 'col'
          "
        >
          <h4 class="text-dark mb-4" v-if="isPlaylistItems">
            Playlist
          </h4>
          <div class="row">
            <div class="col-12 col-lg-3">
              <img
                class="img-fluid"
                :src="item.snippet.thumbnails.medium.url"
                alt="YouTube thumbnail"
              />
            </div>
            <div class="col-12 col-lg-9">
              <h5 class="card-title text-dark">
                {{ item.snippet.title }}
              </h5>
              <h6 class="card-subtitle mb-2 text-muted">
                {{ item.snippet.channelTitle }} |
                {{ item.snippet.publishedAt | formatDate }}
              </h6>
              <p class="card-text text-muted" v-if="item.snippet.description">
                {{ item.snippet.description.slice(0, 250) }}...
              </p>
            </div>
          </div>
        </div>

        <!-- playlis item -->
        <PlaylistItem
          v-if="isPlaylistItems"
          :id="id"
          :isPlaylistItems="isPlaylistItems"
        />
        <div v-if="isChannelSections">
          <h4 class="text-info mb-3">Channel sections</h4>
        </div>
      </div>
    </a>
  </div>
</template>

<script>
import PlaylistItem from "./PlaylistItem";
export default {
  name: "VideoListItem",
  components: { PlaylistItem },
  props: ["item", "param", "id", "isPlaylistItems", "isChannelSections"],
};
</script>
