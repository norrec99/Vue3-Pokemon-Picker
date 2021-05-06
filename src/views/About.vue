<template>
  <p>{{ turnAround }}</p>
  <div class="about">
    <div
      v-if="pokemon"
      class="w-3/12 m-auto bg-purple-100 mt-4 shadow-2xl flex justify-center flex-col items-center"
    >
      <!-- <h3 class="text-2xl text-green-900 uppercase">{{ pokemon.name }}</h3> -->
      <h3 class="text-2xl text-green-900">??</h3>
      <div class="flex justify-center">
        <img
          @click="turnPokemon"
          class="w-48"
          :src="pokemon.sprites.front_shiny"
          alt=""
        />
        <img class="w-48" :src="pokemon.sprites.back_shiny" alt="" />
      </div>
      <h3 class="text-yellow-400">Types</h3>
      <div v-for="(type, idx) in pokemon.types" :key="idx">
        <h5 class="text-blue-900">{{ type.type.name }}</h5>
      </div>
    </div>
    <div class="w-full flex justify-center">
      <input
        type="text"
        placeholder="Who is this Pokémon?"
        class="mt-10 p-2 border-blue-500 border-2"
        v-model="text"
      />

      <p v-if="!isTrue">Hello</p>
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
      text: "",
      isTrue: computed(() => isEqual()),
    });

    function isEqual(text) {
      if (state.text !== text) {
        return false;
      }
      return "correct";
    }

    fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}`)
      .then((res) => res.json())
      .then((data) => {
        console.log(data);
        state.pokemon = data;
      });
    return { ...toRefs(state, isEqual) };
  },
};
</script>
