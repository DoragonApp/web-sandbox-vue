<template>
  <v-row>
    <v-col xs12 sm8 md6>
      <v-card>
        <v-card-title>Anime per season</v-card-title>
        <v-container>
          <p v-if="$fetchState.pending">Fetching seasons...</p>
          <p v-else-if="$fetchState.error">Error while fetching seasons</p>
          <v-row v-else>
            <v-col v-for="year in archive" :key="year.year">
              <div v-for="season in year.seasons" :key="season">
                <v-card
                  class="mb-6"
                  :href="'/season/' + year.year + '/' + season.toLowerCase()"
                >
                  <v-card-title>{{ season + ' ' + year.year }}</v-card-title>
                </v-card>
              </div>
            </v-col>
          </v-row>
        </v-container>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  async fetch() {
    try {
      const response = await fetch(
        `https://api.jikan.moe/v3/season/archive`
      ).then((res) => res.json())
      this.archive = response.archive
    } catch {
      throw new Error('There has been an error. Contact the developer.')
    }
  },
  data() {
    return {
      archive: [],
    }
  },
}
</script>

<style>
.v-card__text,
.v-card__title {
  word-break: normal; /* maybe !important  */
}
</style>
