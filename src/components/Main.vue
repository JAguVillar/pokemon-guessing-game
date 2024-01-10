<template>
  <h1 class="text-center m-4">Busca un pokémon que sea del tipo:</h1>
  <div class="flex justify-center">
    <div v-for="type in types" :key="type" class="m-4">
      <img style="width: 128px" :src="imgRoute(type)" alt="" />
    </div>
  </div>
  <div class="m-4">
    <Filter :initialPokemonData="allPokemon" @choose="log" />
  </div>
  <div>
    {{ "HP: " + hp + "/3" }}
  </div>
  <template v-if="infoPokemon != null">
    <div v-if="infoPokemon.mapped">
      <div
        class="shadow border select-none cursor-pointer bg-white rounded-md flex flex-1 items-center p-4"
      >
        <div class="flex flex-col items-center justify-center mr-4">
          <img
            style="width: 120px"
            :src="infoPokemon.info.sprite"
            class="mx-auto"
          />
        </div>
        <div class="flex-1 pl-1 mr-4">
          <div class="font-big capitalize m-2">
            {{ infoPokemon.info.pokemon }}
          </div>
          <div class="flex">
            <div v-for="type in infoPokemon.info.types" :key="type" class="m-2">
              <img style="width: 72px" :src="imgRoute(type.name)" alt="" />
            </div>
          </div>
          <div class="font-big capitalize m-2">Descripción Pokédex:</div>
          <div class="m-2 flex flex-1">
            <span>
              {{ infoPokemon.mappedData.version }}
            </span>
            <span>
              {{ infoPokemon.mappedData.flavor_text }}
            </span>
          </div>
        </div>
      </div>
    </div>
    <div v-else>
      <div>{{ infoPokemon }}</div>
    </div>
  </template>
</template>

<script>
import Filter from "./Filter.vue";

export default {
  components: {
    Filter,
  },
  props: {
    fetchedData: {
      type: Array,
      required: true,
    },
    uniqueTypes: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      allPokemon: null,
      types: null,
      result: null,
      hp: 3,
      infoPokemon: null,
    };
  },
  watch: {
    hp: function (val) {
      if (val == 0) {
        alert("putardo");
      }
    },
  },
  created() {
    this.allPokemon = this.fetchedData;
    this.types = this.uniqueTypes;
    console.log(this.allPokemon);
  },
  computed: {},
  methods: {
    randomType() {
      let types = this.types[Math.floor(Math.random() * this.types.length)];
      this.types = types;
    },
    imgRoute(name) {
      return "/tipos/" + name + ".png";
    },

    async log(value) {
      let species_response = null;
      console.log(value);

      try {
        if (this.doesPokemonHaveType(value.pokemon, value.types) == false) {
          this.hp--;
        } else {
          species_response = await this.getSpecies(value.pokemon);
        }

        const newArray = species_response.flavor_text_entries.filter(
          (item) => item.language.name === "es"
        );
        const newArrayMapped = newArray.map((item) => ({
          flavor_text: item.flavor_text,
          version: item.version.name,
        }));

        const item = {
          mapped: true,
          mappedData:
            newArrayMapped[Math.floor(Math.random() * newArrayMapped.length)],
          info: value,
        };
        this.infoPokemon = item;
      } catch (error) {
        console.error("Error:", error);
        const item = {
          mapped: false,
          mappedData: null,
          info: value,
        };
        this.infoPokemon = item;
      }
    },

    doesPokemonHaveType(pokemon, types) {
      return types.every((type) => this.types.includes(type.name));
    },
    async getSpecies(pokemon) {
      try {
        const response = await fetch(
          "https://pokeapi.co/api/v2/pokemon-species/" + pokemon
        );

        if (!response.ok) {
          const data = await response.json();
          const error = (data && data.message) || response.statusText;
          return Promise.reject(error);
        }

        const data = await response.json();
        console.log(data);
        return data;
      } catch (error) {
        this.errorMessage = error;
        alert("There was an error!", error);
      }
    },
  },
};
</script>

<style>
/* Estilos CSS */
</style>
