<template>
  <v-container>
    <p v-if="$fetchState.pending">Fetching anime...</p>
    <p v-else-if="$fetchState.error">Error while fetching anime</p>
    <v-row v-else>
      <v-col v-for="anime in animes" :key="anime.mal_id">
        <anime-card
          :url="anime.url"
          :title="anime.title"
          :image_url="anime.image_url"
          :synopsis="anime.synopsis"
          :genres="anime.genres"
          :score="anime.score"
        ></anime-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  async fetch() {
    const response = await fetch(
      `https://api.jikan.moe/v3/season/${this.$route.params.year}/${this.$route.params.season}`
    ).then((res) => res.json())

    if (
      response.season_year === this.$route.params.year ||
      response.season_name.toLowerCase() === this.$route.params.season // Just a workaround, still need a proper way to check
    ) {
      this.animes = response.anime
    } else {
      // set status code on server and
      if (process.server) {
        this.$nuxt.context.res.statusCode = 404
      }
      // use throw new Error()
      throw new Error('There has been an error. Contact the developer.')
    }
  },
  data() {
    return {
      animes: [],
    }
  },
}
</script>
