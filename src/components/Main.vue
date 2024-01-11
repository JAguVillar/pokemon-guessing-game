<template>
  <h1 class="text-center m-4">Busca un pokémon que sea del tipo:</h1>
  <div class="flex justify-center">
    <div v-for="type in types" :key="type" class="m-4">
      <img style="width: 128px" :src="imgRoute(type)" alt="" />
    </div>
  </div>
  <div>
    HP:
    <span :class="hpColor">
      {{ hp + "/3" }}
    </span>
  </div>
  <div class="m-4">
    <Filter :initialPokemonData="allPokemon" @choose="log" />
  </div>

  <dialog class="modal max-w-3xl rounded-xl" id="modalSuccess">
    <div class="w-full flex justify-between">
      <div class="font-big py-2 px-4 m-4">
        Felicidades!, encontraste a un Pokémon correcto!
      </div>
      <button
        type="button"
        class="py-2 px-4 flex justify-center items-center bg-white hover:bg-slate-400 transition ease-in duration-200 text-center text-base font-semibold focus:outline-none w-12 h-12 rounded-lg m-4"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          height="16"
          width="12"
          viewBox="0 0 384 512"
        >
          <path
            d="M342.6 150.6c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0L192 210.7 86.6 105.4c-12.5-12.5-32.8-12.5-45.3 0s-12.5 32.8 0 45.3L146.7 256 41.4 361.4c-12.5 12.5-12.5 32.8 0 45.3s32.8 12.5 45.3 0L192 301.3 297.4 406.6c12.5 12.5 32.8 12.5 45.3 0s12.5-32.8 0-45.3L237.3 256 342.6 150.6z"
          />
        </svg>
      </button>
    </div>
    <div class="flex justify-center">
      <img
        src="../../public/assets/totodile.gif"
        alt=""
        style="width: 50%"
        class="m-4"
      />
    </div>
  </dialog>
  <dialog class="modal max-w-3xl rounded-xl" id="modalFail">
    <div class="w-full flex justify-between">
      <div class="font-big py-2 px-4 m-4">Ya no te quedan más intentos!</div>
      <button
        type="button"
        class="py-2 px-4 flex justify-center items-center bg-white hover:bg-slate-400 transition ease-in duration-200 text-center text-base font-semibold focus:outline-none w-12 h-12 rounded-lg m-4"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          height="16"
          width="12"
          viewBox="0 0 384 512"
        >
          <path
            d="M342.6 150.6c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0L192 210.7 86.6 105.4c-12.5-12.5-32.8-12.5-45.3 0s-12.5 32.8 0 45.3L146.7 256 41.4 361.4c-12.5 12.5-12.5 32.8 0 45.3s32.8 12.5 45.3 0L192 301.3 297.4 406.6c12.5 12.5 32.8 12.5 45.3 0s12.5-32.8 0-45.3L237.3 256 342.6 150.6z"
          />
        </svg>
      </button>
    </div>
    <div class="flex justify-center">
      <img
        src="../../public/assets/pichu.gif"
        alt=""
        style="width: 50%"
        class="m-4"
      />
    </div>
  </dialog>
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
      hpColor: "text-green-600",
      infoPokemon: null,
    };
  },
  watch: {
    hp: function (val) {
      if (val > 0) {
        if (val == 3) {
          this.hpColor = "text-green-600";
        } else if (val == 2) {
          this.hpColor = "text-orange-600";
        } else {
          this.hpColor = "text-red-600";
        }
      } else {
        const modal = document.querySelector("#modalFail");
        modal.showModal();
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
          const modal = document.querySelector("#modalSuccess");
          modal.showModal();
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
