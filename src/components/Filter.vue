<template>
  <div class="select-container mx-auto">
    <input
      class="rounded-lg border-transparent flex-1 appearance-none border border-gray-300 py-2 px-4 bg-blue-50 text-gray-700 placeholder-gray-400 shadow-sm text-base focus:outline-none focus:ring-2 focus:ring-purple-600 focus:border-transparent w-full"
      placeholder="Your email"
      type="text"
      id="pokemon"
      v-model="searchText"
      @input="filterPokemon"
      @focus="openDropdown"
    />
    <ul v-if="showDropdown" class="dropdown w-full">
      <Item
        @choose="log"
        v-for="pokemon in filteredPokemon"
        :key="pokemon"
        :pokemon="pokemon.name"
        :sprite="pokemon.sprite"
        :type="pokemon.types"
      />
    </ul>
  </div>
</template>

<script>
import Item from "./Item.vue";

export default {
  components: { Item },
  props: {
    initialPokemonData: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      searchText: "",
      selectedPokemon: null,
      allPokemon: null,
      showDropdown: false,
    };
  },
  created() {
    this.allPokemon = this.initialPokemonData;
  },
  computed: {
    filteredPokemon() {
      if (this.searchText.length >= 3) {
        return this.allPokemon.filter((pokemon) =>
          pokemon.name.toLowerCase().includes(this.searchText.toLowerCase())
        );
      }
    },
  },
  methods: {
    log(value) {
      this.$emit("choose", value);
    },
    filterPokemon() {
      this.showDropdown = true;
      // Add any additional filtering logic if needed
    },
    openDropdown() {
      this.showDropdown = true;
    },
    closeDropdown() {
      this.showDropdown = false;
    },
    selectPokemon(pokemon) {
      this.selectedPokemon = pokemon;
      this.showDropdown = false;
    },
  },
};
</script>

<style>
/* Estilos CSS */
</style>
