<template>
  <b-card
    :title="`#${pokemon.id} - ${pokemon.name}`"
    :img-src="pokemon.sprites.front_default"
    class="text-capitalize"
  >
    <badges-pokemon-types :types="typesExtracted" />

    <div
      v-for="(stat, i) in pokemon.stats"
      :key="`stat-${i}`"
      class="mt-3"
    >
      <div>{{ stat.stat.name }}</div>
      <b-progress :value="stat.base_stat" :max="255" show-value />
    </div>
  </b-card>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  props: {
    pokemon: {
      type: Object,
      required: true,
    },
  },
  computed: {
    typesExtracted() {
      return this.pokemon.types.map(type => type.type);
    },
  },
})
</script>
