<script setup>
  import {ref} from "vue";

  const searchTerm = ref('')
  const options = ref(['AAA', 'BBB', 'CCC'])
  const filter = (_, update) => update(() => options.value = ['AAA', 'BBB', 'CCC'])

</script>
<template>
<div>
  <q-select
      v-model="searchTerm"
      :options="options"
      use-input
      @filter="filter"
  >
  </q-select>
</div>
</template>