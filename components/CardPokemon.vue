<template>
  <b-card
    :title="`#${pokemon.id} - ${pokemon.name}`"
    :img-src="pokemon.sprites.front_default"
    class="text-capitalize"
  >
    <b-badge
      v-for="type in pokemon.types"
      :key="type.type.name"
      class="mr-1"
    >
      {{ type.type.name }}
    </b-badge>

    <div
      v-for="(stat, i) in pokemon.stats"
      :key="`stat-${i}`"
    >
      <div>{{ stat.stat.name }}</div>
      <b-progress :value="stat.base_stat" :max="255" class="mb-3" show-value />
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
})
</script>
