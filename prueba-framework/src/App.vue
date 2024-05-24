<template>
  <div id="app">
    <div>
      <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/98/International_Pok%C3%A9mon_logo.svg/800px-International_Pok%C3%A9mon_logo.svg.png"
      :style="{ width: '300px' }" />
      <h2>¿Quién ese Pokémon?</h2>
      
    </div>
    <AdivinaPokemon :pokemon-list="pokemonList" @pokemon-discovered="pokemonDiscovered" />
  </div>
</template>

<script>
import AdivinaPokemon from './components/AdivinaPokemon.vue';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    AdivinaPokemon
  },
  data() {
    return {
      pokemonList: [],
      correctGuesses: 0
    };
  },
  methods: {
    pokemonDiscovered() {
      this.correctGuesses++;
    },
    async fetchPokemonData() {
      try {
        const response = await axios.get('https://pokeapi.co/api/v2/pokemon');
        const data = response.data;
        this.pokemonList = data.results.map((pokemon, index) => ({
          name: pokemon.name,
          image: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${index + 1}.png`,
          discovered: false,
          guess: ''
        }));
      } catch (error) {
        console.error('Error fetching Pokémon data:', error);
      }
    }
  },
  mounted() {
    this.fetchPokemonData();
  }
}
</script>

<style>
#app {
  font-family: Arial, sans-serif;
  text-align: center;
}
</style>
