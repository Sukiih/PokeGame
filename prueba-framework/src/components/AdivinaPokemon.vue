<template>
  <div class="pokemon-game">
    <p v-if="correctGuesses > 0" class="counter">Pokemones descubiertos: {{ correctGuesses }}</p>
    <div v-for="(pokemon, index) in pokemonList" :key="index" class="pokemon-item">
      <img :src="getPokemonImage(index)"
           :style="{ filter: pokemon.discovered ? 'none' : 'blur(3px) grayscale(100%)' }"
           class="silhouette" />
      <div v-if="!pokemon.discovered">
        <input type="text" v-model="pokemon.guess" :placeholder="'Adivina el Pokémon ' + (index + 1)" />
        <button @click="checkPokemon(index)">Descubrir</button>
      </div>
      <p v-else>{{ pokemon.name }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AdivinaPokemon',
  data() {
    return {
      pokemonList: [],
      correctGuesses: 0,
    };
  },
  mounted() {
    this.fetchPokemonData();
  },
  methods: {
    async fetchPokemonData() {
      try {
        const response = await fetch('https://pokeapi.co/api/v2/pokemon');
        const data = await response.json();
        this.pokemonList = data.results.map(pokemon => ({
          name: pokemon.name,
          discovered: false,
          guess: ''
        }));
      } catch (error) {
        console.error('Error fetching Pokémon data:', error);
      }
    },
    getPokemonImage(index) {
      return `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${index + 1}.png`;
    },
    checkPokemon(index) {
      const currentPokemonName = this.pokemonList[index].name;
      if (this.pokemonList[index].guess.toLowerCase() === currentPokemonName) {
        this.pokemonList[index].discovered = true;
        this.$emit('pokemon-discovered');
        this.correctGuesses++;
      } else {
        alert('Nombre incorrecto. ¡Sigue intentando!');
      }
    },
  },
}
</script>

<style>
.pokemon-game {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.pokemon-item {
  margin: 10px;
  text-align: center;
}

</style>
