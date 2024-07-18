<!-- Rendrerizamos la tarjeta de pokemon usando estilos de Bootstrap para colocarlos en forma de grid -->

<template>
  <div class="col mb-4 me-2">
    <div class="card">
      <!-- Agregamos los props que buscamos en el objeto llamando a la Api de Pokemon -->
      <img
        :src="imageUrl"
        class="card-img-top"
        :alt="pokemonName"
        :style="cardStyle"
      />
      <div class="card-body">
        <!-- Hacemos binding del input de Pokemon y agregamos el metodo para revisar Pokemon al hacer enter -->
        <input
          v-model="inputValue"
          @keydown.enter="checkPokemon"
          v-if="!discovered"
          class="form-control mb-2"
          placeholder="Qué Pokémon es?"
        />
        <!-- Agregamos boton para revisar si el nombre ingresado existe -->
        <button
          @click="checkPokemon"
          v-if="!discovered"
          class="btn btn-primary"
        >
          Descubrir
        </button>
        <p v-if="discovered" class="card-text">{{ pokemonName }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    pokemonName: String,
    imageUrl: String,
  },
  data() {
    return {
      inputValue: "",
      discovered: false,
    };
  },
  computed: {
    cardStyle() {
      return this.discovered
        ? {}
        : {
            filter: "blur(5px) grayscale(100%)",
          };
    },
  },
  methods: {
    checkPokemon() {
      if (
        this.inputValue.trim().toLowerCase() === this.pokemonName.toLowerCase()
      ) {
        this.discovered = true;
        this.$emit("pokemon-discovered");
      } else {
        alert("Nombre incorrecto. Inténtalo de nuevo.");
      }
    },
  },
};
</script>

<style scoped>
/* Estilos locales para el componente PokemonCard  */
.card {
  width: 200px;
  text-align: center;
}

.card-img-top {
  width: 100%;
  height: 200px;
  object-fit: cover;
}
</style>
