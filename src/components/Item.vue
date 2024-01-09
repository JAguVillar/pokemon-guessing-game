<template>
    <div
        class="container flex flex-col items-center justify-center w-full mx-auto bg-white rounded-lg shadow dark:bg-gray-800"
    >
        <ul class="flex flex-col divide-y divide">
            <li class="flex flex-row" @click="log(pokemon, type)">
                <div
                    class="flex items-center flex-1 p-4 cursor-pointer select-none"
                >
                    <div
                        class="flex flex-col items-center justify-center w-10 h-10 mr-4"
                    >
                        <img
                            style="width: 120px"
                            alt="profil"
                            :src="sprite"
                            class="mx-auto"
                        />
                    </div>
                    <div class="flex-1 pl-1">
                        <div
                            class="fuente font-medium dark:text-white capitalize"
                        >
                            {{ pokemon }}
                        </div>
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
