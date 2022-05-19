<template>
  <div>
    <b-badge
      v-for="type in types"
      :key="type.url"
      class="mr-1 text-capitalize"
    >
      {{ type.name }}
    </b-badge>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  props: {
    types: {
      type: Array,
      default: () => [],
    },
  },
})
</script>
