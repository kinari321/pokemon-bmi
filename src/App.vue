<script setup>
  import { ref, onMounted } from 'vue';

  const pokemonName = ref('');
  const pokemonData = ref(null);

  async function fetchPokemonData() {
    try {
      const res = await fetch(
        `https://pokeapi.co/api/v2/pokemon/${pokemonName.value}`
      );
      if (!res.ok) {
        throwError(res);
      }
      pokemonData.value = await res.json();
    } catch (err) {
      console.error('Failed to fetch Pokemon data:', err);
    }

    pokemonName.value = '';
  }

  function throwError(err) {
    throw new Error(`エラーが発生しました！\n  ステータスコード${err.status}`);
  }
</script>

<template>
  <div>
    <p>ポケモンの英語名または番号を入力してください。</p>
    <p>
      ポケモンの番号については<a
        href="https://wiki.xn--rckteqa2e.com/wiki/%E3%83%9D%E3%82%B1%E3%83%A2%E3%83%B3%E4%B8%80%E8%A6%A7"
        target="_blank"
        rel="noopener noreferrer"
        >こちら</a
      >を参照
    </p>
  </div>
  <input v-model="pokemonName" placeholder="Type here" />
  <p>{{ pokemonName }}</p>
  <button @click="fetchPokemonData">ポケモンを検索</button>
  <!-- <p v-if="!pokemonData">{{ loadingText }}</p> -->
  <!-- <pre v-else>{{ pokemonData }}</pre> -->
  <pre>{{ pokemonData }}</pre>
</template>
