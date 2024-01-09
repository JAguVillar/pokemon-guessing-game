<template>
  <li
    class="my-4 flex flex-row bg-slate-500 w-full rounded-xl"
    @click="log(pokemon, type)"
  >
    <div class="flex items-center flex-1 cursor-pointer select-none">
      <div class="flex flex-col items-center mr-4">
        <img style="width: 120px" alt="profil" :src="sprite" class="mx-auto" />
      </div>
      <div class="flex-1 pl-1">
        <div class="fuente font-medium capitalize">
          {{ pokemon }}
        </div>
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
  },
  computed: {},
  methods: {
    log(p, t) {
      this.$emit("choose", { pokemon: p, types: t });
    },
  },
};
</script>

<style>
/* Estilos CSS */
@font-face {
  font-family: "Pkmn";
  src: url("/fonts/Pkmn.ttf") format("truetype");
}

* {
  font-family: "Pkmn", sans-serif;
}
</style>
