<template>
  <b-container>
    <b-row cols="4">
      <b-col
        v-for="(pokemon, i) in pokemons"
        :key="pokemon.url"
        class="mt-4"
      >
        <nuxt-link :to="pokemon.name" class="text-decoration-none text-body">
          <b-card
            :title="`#${i+1}`"
            :img-src="getOfficialArtwork(i+1)"
            class="text-capitalize"
          >
            {{ pokemon.name }}
          </b-card>
        </nuxt-link>
      </b-col>
    </b-row>
    <b-row class="mt-4">
      <b-col class="text-center">
        <b-spinner v-if="isLoading" type="grow" />
      </b-col>
    </b-row>
  </b-container>
</template>

<script lang="ts">
import Vue from 'vue'
import { URL_BASE_POKEAPI, URL_BASE_OFFICIAL_ARTWORK } from '~/config';

const DEFAULT_LIMIT = 48;
const SCROLL_BOTTOM_DISTANCE = 50;

export default Vue.extend({
  name: 'IndexPage',
  data: () => ({
    isLoading: false,
    pokemons: [] as any,
    next: '',
    currentOffset: 0,
  }),
  created() {
    this.getPokemonList();
  },
  mounted() {
    this.attachScrollListener();
  },
  destroyed() {
    this.detachScrollListener();
  },
  methods: {
    async getPokemonList() {
      if (this.isLoading) return;

      this.isLoading = true;
      try {
        const { next, results } = await this.$axios.$get(`${URL_BASE_POKEAPI}/pokemon-species`, {
          params: {
            limit: DEFAULT_LIMIT,
            offset: this.currentOffset,
          },
        });

        this.pokemons = [...this.pokemons, ...results];
        this.next = next;
        this.currentOffset += DEFAULT_LIMIT;
      } catch (error) {

      } finally {
        this.isLoading = false;
      }
    },
    getOfficialArtwork(index: number) {
      return `${URL_BASE_OFFICIAL_ARTWORK}/${index}.png`;
    },
    onPageScroll()  {
      const { pageYOffset, innerHeight } = global;
      const { offsetTop, offsetHeight } = document.documentElement;

      const bottomWindowOffset = pageYOffset + innerHeight;
      const bottomTabLimit = offsetTop + offsetHeight - SCROLL_BOTTOM_DISTANCE;
      const reachedBottom = bottomWindowOffset >= bottomTabLimit;

      if (reachedBottom) {
        this.getPokemonList();
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
