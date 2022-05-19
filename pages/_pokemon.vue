<template>
  <b-container class="p-4">
    <b-nav>
      <b-nav-item>
        <nuxt-link to="/">Back</nuxt-link>
      </b-nav-item>
    </b-nav>
    <b-row>
      <b-col v-for="pokemon in pokemonDetails" :key="`col-${pokemon.id}`">
        <nuxt-link :to="pokemon.name" class="text-decoration-none text-body">
          <card-pokemon
            :pokemon="pokemon"
            :is-active="pokemon.name === params.pokemon"
            class="mx-auto"
            style="width: 320px;"
          />
        </nuxt-link>
      </b-col>
    </b-row>
  </b-container>
</template>

<script lang="ts">
import Vue from 'vue'
import { URL_BASE_POKEAPI } from '~/config';

let temp: any[] = [];

const getSpeciesChain = (chain: any) => {
  temp.push(chain.species.name);

  if (chain.evolves_to.length) {
    getSpeciesChain(chain.evolves_to[0])
  }

  return temp;
}

export default Vue.extend({
  async asyncData({ $axios, params }) {
    temp = [];
    const species = await $axios.$get(`${URL_BASE_POKEAPI}/pokemon-species/${params.pokemon}`);
    const evolutionChain = await $axios.$get(species.evolution_chain.url);
    const speciesChain = getSpeciesChain(evolutionChain.chain);
    const pokemonDetails = await Promise.all(speciesChain.map(species => $axios.$get(`${URL_BASE_POKEAPI}/pokemon/${species}`)));

    return {
      params,
      species,
      speciesChain,
      pokemonDetails,
    };
  },
})
</script>
