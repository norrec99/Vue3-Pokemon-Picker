<template>
  <div class="w-full flex justify-center">
    <input
      type="text"
      placeholder="Enter Pokemon here"
      class="mt-10 p-2 border-blue-500 border-2"
      v-model="text"
    />
  </div>

  <h1 class="w-full flex justify-center mt-5">Or...</h1>

  <div class="w-full flex justify-center">
    <router-link :to="`/about/${randNum}`">
      <button
        class="bg-gradient-to-r from-green-400 to-blue-500 hover:from-pink-500 hover:to-yellow-500 p-2 mt-5 h-10"
      >
        Random 1st Gen Pokemon
      </button>
    </router-link>
  </div>

  <div class="mt-10 p-4 flex flex-wrap justify-center">
    <div
      class="ml-4 text-2x text-blue-500"
      v-for="(pokemon, idx) in filteredPokemon"
      :key="idx"
    >
      <router-link :to="`/about/${urlIdLookup[pokemon.name]}`">
        {{ pokemon.name }}
      </router-link>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import { reactive, toRefs, computed } from "vue";

export default {
  name: "Home",
  setup() {
    const state = reactive({
      pokemons: [],
      urlIdLookup: {},
      text: "",
      num: 151,
      filteredPokemon: computed(() => updatePokemon()),
      randNum: computed(() => randomNumber()),
    });

    function randomNumber() {
      return Math.floor(Math.random() * state.num + 1);
    }

    function updatePokemon() {
      if (!state.text) {
        return [];
      }
      return state.pokemons.filter((pokemon) =>
        pokemon.name.includes(state.text)
      );
    }

    fetch(`https://pokeapi.co/api/v2/pokemon/?offset=0&limit=${state.num}`)
      .then((res) => res.json())
      .then((data) => {
        console.log(data);
        state.pokemons = data.results;
        state.urlIdLookup = data.results.reduce(
          (acc, cur, idx) => (acc = { ...acc, [cur.name]: idx + 1 }),
          {}
        );
      });
    return { ...toRefs(state) };
  },
};
</script>
