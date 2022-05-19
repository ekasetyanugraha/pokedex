<template>
  <b-card
    :title="`#${pokemon.id} - ${pokemon.name}`"
    :img-src="getOfficialArtwork(pokemon.id)"
    :border-variant="isActive ? 'primary' : ''"
    class="text-capitalize"
  >
    <badges-pokemon-types :types="typesExtracted" />

    <div v-if="isActive">
      <div
        v-for="(stat, i) in pokemon.stats"
        :key="`stat-${i}`"
        class="mt-3"
      >
        <div>{{ stat.stat.name }}</div>
        <b-progress :value="stat.base_stat" :max="255" show-value />
      </div>
    </div>
  </b-card>
</template>

<script lang="ts">
import Vue from 'vue'
import { URL_BASE_OFFICIAL_ARTWORK } from '~/config';

export default Vue.extend({
  props: {
    pokemon: {
      type: Object,
      required: true,
    },
    isActive: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    typesExtracted() {
      return this.pokemon.types.map((type: { type: any; }) => type.type);
    },
  },
  methods: {
    getOfficialArtwork(index: number) {
      return `${URL_BASE_OFFICIAL_ARTWORK}/${index}.png`;
    },
  },
})
</script>
