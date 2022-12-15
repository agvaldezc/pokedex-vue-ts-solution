<script setup lang="ts">
import PokemonCard from './PokemonCard.vue';
import type { IPokemon } from '@/interfaces/Pokemon.interface';
import { getPokemon } from '@/services/pokedex.service';
import { computed, ref, type ComputedRef, type Ref } from 'vue';

const pokemon: Ref<IPokemon | null> = ref(null);
const error: Ref<any> = ref(null);
const query: Ref<string> = ref('');

const isValidQuery: ComputedRef = computed(() => /^[a-zA-z0-9]+$/.test(query.value))

function searchPokemon(): void {
  getPokemon(query.value).then((res: IPokemon) => {
    pokemon.value = res;
    error.value = null;
  })
  .catch((err) => {
    error.value = err;
    pokemon.value = null;
  });
}
</script>

<template>
  <section className="pokedex">
    <form @submit.prevent="searchPokemon" className="pokedex-search-form">
      <h2>Pokedex Component</h2>
      <fieldset>
        <input v-model="query" name="query" type="text" placeholder="Pokemon name or ID" />
        <button :disabled="!isValidQuery" type="submit">Search Pokemon</button>
      </fieldset>
    </form>
    <article className="pokedex-container">
      <PokemonCard :pokemon="pokemon" :error="error" />
    </article>
  </section>
</template>

<style scoped>
fieldset {
  border: none;
}

.pokedex {
  display: flex;
  flex-flow: column;
}

.pokedex-search-form {
  display: flex;
  flex-flow: column;
  gap: 16px;
}
</style>
