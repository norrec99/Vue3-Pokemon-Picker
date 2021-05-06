<template>
  <div class="about">
    <div
      v-if="pokemon"
      class="w-3/12 m-auto bg-purple-100 mt-4 shadow-2xl flex justify-center flex-col items-center"
    >
      <h3 v-if="isTrue" class="text-2xl text-green-900 uppercase">
        {{ pokemon.name }}
      </h3>
      <h3 v-else class="text-2xl text-green-900">??</h3>
      <div class="flex justify-center">
        <img class="w-48" :src="pokemon.sprites.front_shiny" alt="" />
        <img class="w-48" :src="pokemon.sprites.back_shiny" alt="" />
      </div>
      <h3 class="text-yellow-400">Types</h3>
      <div v-for="(type, idx) in pokemon.types" :key="idx">
        <h5 class="text-blue-900">{{ type.type.name }}</h5>
      </div>
    </div>
    <div class="w-full flex justify-center mb-2">
      <input
        type="text"
        placeholder="Who is this Pokémon?"
        class="mt-10 p-2 border-blue-500 border-2"
        v-model="text"
      />
    </div>
    <div class="w-full flex justify-center">
      <p v-if="isTrue">Ahh! Another Pokémon nerd is here...</p>
      <p v-else-if="text === ''">Guess the name</p>
      <p v-else>Not quite right...</p>
    </div>
    <div class="w-full flex justify-center">
      <button
        class="bg-gradient-to-r from-green-400 to-blue-500 hover:from-pink-500 hover:to-yellow-500 mt-5 h-10"
        @click="getName"
      >
        Just tell me!
      </button>
    </div>
  </div>
</template>

<script>
import { useRoute } from "vue-router";
import { reactive, toRefs, computed } from "vue";
export default {
  setup() {
    const route = useRoute();
    const state = reactive({
      pokemon: null,
      pokemonName: [],
      text: "",
      isTrue: computed(() => isEqual()),
      getName: computed(() => giveMeName()),
    });

    function isEqual() {
      if (state.text !== state.pokemonName) {
        return false;
      }
      return true;
    }

    function giveMeName() {
      return (state.text = state.pokemonName);
    }

    fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}`)
      .then((res) => res.json())
      .then((data) => {
        // console.log(data);
        state.pokemon = data;
        state.pokemonName = data.name;
      });
    return { ...toRefs(state) };
  },
};
</script>
