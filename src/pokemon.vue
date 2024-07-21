<script setup>
  import { ref } from 'vue';

  const pokemonName = ref('');
  const pokemon = ref(null);
  const pokemons = defineProps({
    pokemons: Number,
  });

  async function fetchPokemon() {
    try {
      const res = await fetch(
        `https://pokeapi.co/api/v2/pokemon/${pokemonName.value}`
      );
      if (!res.ok) {
        throwError(res);
      }
      pokemon.value = await res.json();
      console.log(pokemon.value);
      pokemons.value.push({ id: id++, data: pokemon.value });
      console.log(pokemons);
    } catch (err) {
      console.error('Failed to fetch Pokemon data:', err);
    }

    // pokemon.value = null;
    // pokemonName.value = '';
  }

  function throwError(err) {
    throw new Error(`エラーが発生しました！\n  ステータスコード${err.status}`);
  }
</script>

<template>
  <p>ポケモンの英語名または番号を入力してください。</p>
  <input v-model="pokemonName" placeholder="Type here" />
  <button @click="fetchPokemon">ポケモンを検索</button>
  <p v-if="!pokemon"></p>
  <div v-else>
    <p>名前 : {{ pokemon.name }}</p>
    <p>高さ : {{ pokemon.height / 10 }} m</p>
    <p>重さ : {{ pokemon.weight / 10 }} kg</p>
    <p>
      BMI値 :{{ (pokemon.weight / (pokemon.height * pokemon.height)) * 10 }}
    </p>
    <p>
      ポケモン番号 : No.{{ pokemon.id }}
      （ポケモン図鑑は
      <a
        :href="`https://zukan.pokemon.co.jp/detail/${pokemon.id}`"
        target="_blank"
        rel="noopener noreferrer"
        >こちら</a
      >）
    </p>
    <!-- <pre>{{ pokemon }}</pre> -->
  </div>
</template>
