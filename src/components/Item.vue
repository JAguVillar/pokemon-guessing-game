<template>
  <li class="my-4 flex flex-row bg-[#d1d1e9] w-full rounded-xl px-4">
    <div class="flex items-center flex-1 cursor-pointer select-none">
      <div class="flex flex-col items-center relative">
        <img
          v-show="is_shiny"
          style="width: 120px"
          alt="profil"
          src="../../public/assets/shiny.gif"
          class="mx-auto absolute"
        />
        <img style="width: 120px" alt="profil" :src="sprite" class="mx-auto" />
      </div>
      <div class="flex-1 pl-1">
        <div class="capitalize" style="font-size: 32px">
          {{ pokemon }}
        </div>
      </div>
      <div class="flex flex-col items-center">
        <button
          @click="log(pokemon, type, sprite)"
          style="font-size: 32px"
          type="button"
          class="py-2 px-4 bg-[#6246ea] hover:bg-indigo-700 focus:ring-indigo-500 focus:ring-offset-indigo-200 text-white w-full transition ease-in duration-200 text-center shadow-md focus:outline-none focus:ring-2 focus:ring-offset-2 rounded-lg"
        >
          Elegir
        </button>
      </div>
    </div>
  </li>
</template>

<script>
export default {
  props: {
    pokemon: {
      type: String,
      required: true,
    },
    sprite: {
      type: String,
      required: true,
    },
    type: {
      type: Array,
      required: true,
    },
    is_shiny: {
      type: Boolean,
      required: true,
    },
  },
  data() {
    return {
      searchText: "",
      selectedPokemon: null,
      allPokemon: null,
    };
  },
  created() {
    // this.allPokemon = this.initialPokemonData;
    console.log(this.is_shiny);
  },
  computed: {},
  methods: {
    log(p, t, s) {
      this.$emit("choose", { pokemon: p, types: t, sprite: s });
    },
  },
};
</script>

<style>
/* Estilos CSS */
@font-face {
  font-family: "Pkmn";
  src: url("/fonts/pokemon-b-w.otf") format("truetype");
}

* {
  font-family: "Pkmn", sans-serif;
}
</style>
