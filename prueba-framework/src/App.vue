<template>
  <div class="background"></div>
  <div id="app">
    <div>
      <!-- Logo Pokémon -->
      <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/98/International_Pok%C3%A9mon_logo.svg/800px-International_Pok%C3%A9mon_logo.svg.png"
           :style="{ width: '300px', marginTop: '150px'}" />
      
      <h2 :style="{marginTop: '70px', marginBottom:'100px', textShadow:'0 5px 10px red'}">¿Quién es ese Pokémon?</h2>
    </div>
    <!-- Componente -->
    <AdivinaPokemon :pokemon-list="pokemonList" @pokemon-descubierto="handlePokemonDescubierto" />
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
      correctGuesses: 0 // Contador 
    };
  },
  methods: {
    // Manejar el evento de descubrimiento de Pokémon
    handlePokemonDescubierto(index) {
      this.pokemonList[index].descubierto = true;
      this.correctGuesses++;
    },
    // Obtener lista de Pokémon
    async fetchPokemonData() {
      try {
        const response = await axios.get('https://pokeapi.co/api/v2/pokemon');
        const data = response.data;
        this.pokemonList = data.results.map((pokemon, index) => ({
          name: pokemon.name, 
          image: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${index + 1}.png`,
          descubierto: false,
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
body{
  margin: 0;
  padding: 0;
}
#app {
  font-family: Arial, sans-serif;
  text-align: center;
  background-image: url(https://favoredexhome.files.wordpress.com/2018/12/Pokeball.png);
  background-repeat: no-repeat;
  background-position:top;
  background-size: 25%;
}


</style>
