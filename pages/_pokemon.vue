<template>
  <b-container class="p-4">
    <card-pokemon :pokemon="pokemon" class="mx-auto" style="width: 250px;" />
  </b-container>
</template>

<script lang="ts">
import Vue from 'vue'
import { URL_BASE_POKEAPI } from '~/config';

export default Vue.extend({
  async asyncData({ $axios, params }) {
    const pokemon = await $axios.$get(`${URL_BASE_POKEAPI}/pokemon/${params.pokemon}`);
    return { pokemon };
  },
})
</script>
