<template>
  <b-container>
    <b-row class="mt-4">
      <b-col>
        Filter by type:
        <badges-pokemon-types :types="types" />
      </b-col>
    </b-row>
    <b-row cols="4">
      <b-col
        v-for="(pokemon, i) in pokemons"
        :key="pokemon.url"
        class="mt-4"
      >
        <nuxt-link :to="pokemon.name">
          <b-card :title="`#${i+1}`" class="text-capitalize">{{ pokemon.name }}</b-card>
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
    isLoading: false,
    pokemons: [] as any,
    types: [] as any,
    next: '',
  }),
  created() {
    this.getPokemonList();
    this.getTypes();
  },
  mounted() {
    this.attachScrollListener();
  },
  destroyed() {
    this.detachScrollListener();
  },
  methods: {
    async getTypes() {
      try {
        const { results } = await this.$axios.$get(`${URL_BASE_POKEAPI}/type`);

        this.types = results;
      } catch (error) {

      }
    },
    async getNextPokemonList() {
      if (!this.next || this.isLoading) return;

      this.isLoading = true;
      try {
        const { next, results } = await this.$axios.$get(this.next);

        this.pokemons = [...this.pokemons, ...results];
        this.next = next;
      } catch (error) {

      } finally {
        this.isLoading = false;
      }
    },
    async getPokemonList() {
      if (this.isLoading) return;

      this.isLoading = true;
      try {
        const { next, results } = await this.$axios.$get(`${URL_BASE_POKEAPI}/pokemon`, {
          params: {
            limit: 50,
            offset: 0,
          },
        });

        this.pokemons = results;
        this.next = next;
      } catch (error) {

      } finally {
        this.isLoading = false;
      }
    },
    onPageScroll()  {
      const { pageYOffset, innerHeight } = global;
      const { offsetTop, offsetHeight } = document.documentElement;

      const bottomWindowOffset = pageYOffset + innerHeight;
      const bottomTabLimit = offsetTop + offsetHeight - 50;
      const reachedBottom = bottomWindowOffset >= bottomTabLimit;

      if (reachedBottom) {
        this.getNextPokemonList();
      }
    },
    attachScrollListener()  {
      global.addEventListener('scroll', this.onPageScroll);
    },
    detachScrollListener()  {
      global.removeEventListener('scroll', this.onPageScroll);
    },
  },
})
</script>
