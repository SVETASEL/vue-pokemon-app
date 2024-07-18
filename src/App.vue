<template>
  <div class="container mt-4 text-center">
    <img
      src="./assets/International_Pokémon_logo.svg.png"
      alt="Pokemon Logo"
      style="width: 500px"
    />
    <h3>Que es ese Pokemon?</h3>
    <h4 class="mb-3">Pokemones descubiertos: ({{ discoveredCount }})</h4>
    <div class="row row-cols-1 row-cols-md-4 g-4">
      <pokemon-card
        v-for="pokemon in pokemons"
        :key="pokemon.id"
        :pokemon-name="pokemon.name"
        :image-url="pokemon.imageUrl"
        @pokemon-discovered="handlePokemonDiscovered"
      ></pokemon-card>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import PokemonCard from "./components/PokemonCard.vue";

export default {
  components: {
    PokemonCard,
  },
  data() {
    return {
      pokemons: [],
      discoveredCount: 0,
    };
  },
  mounted() {
    this.fetchPokemons();
  },
  methods: {
    async fetchPokemons() {
      try {
        const response = await axios.get("https://pokeapi.co/api/v2/pokemon");
        const results = response.data.results.slice(0, 20);
        this.pokemons = results.map((pokemon, index) => ({
          id: index + 1,
          name: pokemon.name,
          imageUrl: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${
            index + 1
          }.png`,
        }));
      } catch (error) {
        console.error("Error al obtener los Pokémon:", error);
      }
    },
    handlePokemonDiscovered() {
      this.discoveredCount++;
    },
  },
};
</script>

<style></style>
