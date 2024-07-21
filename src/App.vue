<script setup>
  import { ref, onMounted } from 'vue';

  let id = 0;
  const pokemonName = ref('');
  const pokemon = ref(null);
  const pokemons = ref([]);

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
  <div>
    <p>ポケモンの英語名または番号を入力してください。</p>
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
  <input v-model="pokemonName" placeholder="Type here" />
  <button @click="fetchPokemon">ポケモンを検索</button>
  <ul>
    <li v-for="pokemon in pokemons" :key="pokemon.id">
      <p v-if="!pokemon"></p>
      <div v-else>
        <p>名前 : {{ pokemon?.data.name }}</p>
        <p>高さ : {{ pokemon?.data.height / 10 }} m</p>
        <p>重さ : {{ pokemon?.data.weight / 10 }} kg</p>
        <p>
          BMI値 :{{
            (pokemon?.data.weight /
              (pokemon?.data.height * pokemon?.data.height)) *
            10
          }}
        </p>
        <p>
          ポケモン番号 : No.{{ pokemon?.data.id }}
          （ポケモン図鑑は
          <a
            :href="`https://zukan.pokemon.co.jp/detail/${pokemon?.data.id}`"
            target="_blank"
            rel="noopener noreferrer"
            >こちら</a
          >）
        </p>
        <!-- <pre>{{ pokemon }}</pre> -->
      </div>
    </li>
  </ul>
</template>
