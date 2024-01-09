<template>
    <Filter :initialPokemonData="allPokemon" @choose="log" />
    <div v-for="type in types" :key="type">
        <img style="width: 128px" :src="imgRoute(type)" alt="" />
    </div>
    <div v-if="result != null">
        {{ result }}
    </div>
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
        };
    },
    created() {
        this.allPokemon = this.fetchedData;
        this.types = this.uniqueTypes;

        // this.randomType();
    },
    computed: {},
    methods: {
        randomType() {
            let types =
                this.types[Math.floor(Math.random() * this.types.length)];
            this.types = types;
        },
        imgRoute(name) {
            return "/tipos/" + name + ".png";
        },
        log(value) {
            let hasChosenTypes = this.doesPokemonHaveType(
                value.pokemon,
                value.types
            );
            this.result = hasChosenTypes;
        },
        doesPokemonHaveType(pokemon, types) {
            return types.every((type) => this.types.includes(type.name));
        },
    },
};
</script>

<style>
/* Estilos CSS */
</style>
