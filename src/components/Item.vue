<template>
  <div
    class="container flex flex-col items-center justify-center w-full mx-auto bg-white rounded-lg shadow dark:bg-gray-800"
  >
    <ul class="flex flex-col divide-y divide">
      <li class="flex flex-row" @click="log(pokemon)">
        <div class="flex items-center flex-1 p-4 cursor-pointer select-none">
          <div class="flex flex-col items-center justify-center w-10 h-10 mr-4">
            <img
              alt="profil"
              :src="sprite"
              class="mx-auto object-cover rounded-full h-10 w-10"
            />
          </div>
          <div class="flex-1 pl-1">
            <div class="font-medium dark:text-white">
              {{ pokemon }}
            </div>
            <!-- <div class="text-sm text-gray-600 dark:text-gray-200">
                        Developer
                    </div> -->
          </div>
        </div>
      </li>
    </ul>
  </div>
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
    log(p) {
      console.log(p);
      this.$emit("choose", p);
    },
  },
};
</script>

<style>
/* Estilos CSS */
</style>
