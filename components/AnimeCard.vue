<template>
  <v-card class="mx-auto" max-width="344">
    <v-img :src="image_url"></v-img>

    <v-card-title>{{ title }}</v-card-title>

    <v-card-subtitle>
      <v-row align="center" class="mx-0">
        <v-rating
          :value="score / 2"
          color="amber"
          dense
          half-increments
          readonly
          size="14"
        ></v-rating>

        <div class="grey--text ml-4">{{ score }}</div>
      </v-row>
    </v-card-subtitle>

    <v-card-text>
      <v-chip
        v-for="genre in genres"
        :key="genre.mal_id"
        class="mr-2 my-2"
        :href="genre.url"
      >
        {{ genre.name }}
      </v-chip>
    </v-card-text>

    <v-card-actions>
      <v-btn color="blue lighten-2" text :href="url"> Link </v-btn>

      <v-spacer></v-spacer>

      <v-btn icon @click="show = !show">
        <v-icon>{{ show ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
      </v-btn>
    </v-card-actions>

    <v-expand-transition>
      <div v-show="show">
        <v-divider></v-divider>

        <v-card-text>
          {{ synopsis }}
        </v-card-text>
      </div>
    </v-expand-transition>
  </v-card>
</template>

<script>
export default {
  props: {
    url: {
      type: String,
      required: false,
      default: '/',
    },
    title: {
      type: String,
      required: true,
    },
    image_url: {
      type: String,
      required: false,
    },
    synopsis: {
      type: String,
      required: false,
      default: 'No synopsis found.',
    },
    airing_start: Date,
    episodes: {
      type: Number,
    },
    genres: {
      type: Array,
      default: () => [],
    },
    source: {
      type: String,
    },
    score: {
      type: Number,
      default: NaN,
    },
  },
  data() {
    return {
      show: false,
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
