<script setup>
  import { ref } from 'vue';
  import Pokemon from './pokemon.vue';

  let id = 0;
  const pokemons = ref([]);

  function addPokemon() {
    pokemons.value.push({}); // 新しい空のオブジェクトを追加
  }
</script>

<template>
  <div>
    <h2>ポケモンBMI</h2>
    <div>
      ポケモン番号については
      <a
        href="https://wiki.xn--rckteqa2e.com/wiki/%E3%83%9D%E3%82%B1%E3%83%A2%E3%83%B3%E4%B8%80%E8%A6%A7"
        target="_blank"
        rel="noopener noreferrer"
        >こちら</a
      >を参照
    </div>
  </div>
  <button @click="addPokemon">比較するポケモンを追加</button>
  <div v-for="pokemon in pokemons" :key="pokemon.id">
    <Pokemon :pokemonId="pokemon.id" />
  </div>
</template>
