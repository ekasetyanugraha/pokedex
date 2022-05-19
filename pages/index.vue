<template>
  <b-container class="p-4">
    <b-row cols="4">
      <b-col
        v-for="(pokemon, i) in pokemons"
        :key="pokemon.url"
        class="mt-4"
      >
        <nuxt-link :to="pokemon.name">
          <b-card :title="`#${i}`" class="text-capitalize">{{ pokemon.name }}</b-card>
        </nuxt-link>
      </b-col>
    </b-row>
  </b-container>
</template>

<script lang="ts">
import Vue from 'vue'
import { URL_BASE_POKEAPI } from '~/config';

export default Vue.extend({
  name: 'IndexPage',
  data: () => ({
    pokemons: [],
  }),
  created() {
    this.getPokemonList();
  },
  methods: {
    async getPokemonList() {
      try {
        const { next, results } = await this.$axios.$get(`${URL_BASE_POKEAPI}/pokemon`);
        this.pokemons = results;
      } catch (error) {

      }
    },
  },
})
</script>
