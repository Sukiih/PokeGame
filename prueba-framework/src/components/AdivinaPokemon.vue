<template>
  
  <!-- Contador-->
  <span class="counter">Pokemones descubiertos: {{ correctGuesses }}</span>
  <div class="pokemon-game">
    
    <!-- Itera sobre la lista de Pokémon -->
    <div v-for="(pokemon, index) in pokemonList" :key="index" class="pokemon-item">
      <!-- Imagen del Pokémon, con filtro de desenfoque y escala de grises si aún no ha sido descubierto -->
      <img :src="getPokemonImage(index)" :style="{ filter: pokemon.discovered ? 'none' : 'blur(2px) grayscale(100%)' }" />
      <!-- Contenedor para mantener el tamaño consistente -->
      <div class="pokemon-details">
        <!-- Campo de entrada para adivinar el nombre del Pokémon -->
        <input v-if="!pokemon.discovered" type="text" v-model="pokemon.guess" placeholder="Adivina el Pokémon" />
        <!-- Botón para descubrir el Pokémon -->
        <button v-if="!pokemon.discovered" @click="checkPokemon(index)">Descubrir</button>
        <!-- Si el Pokémon ya ha sido descubierto, muestra su nombre -->
        <p v-else>{{ pokemon.name }}</p>
      </div>
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
    // Al cargar el componente, se obtiene la data de los Pokémon
    this.fetchPokemonData();
  },
  methods: {
    async fetchPokemonData() {
      try {
        // Se obtiene la lista de Pokémon desde la API PokeAPI
        const response = await fetch('https://pokeapi.co/api/v2/pokemon?limit=20');
        const data = await response.json();
        // Se crea una lista de Pokémon con sus nombres, estado de descubrimiento y campo de adivinanza
        this.pokemonList = data.results.map((pokemon, index) => ({
          name: pokemon.name,
          discovered: false,
          guess: '',
          image: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${index + 1}.png`
        }));
      } catch (error) {
        console.error('Error fetching Pokémon data:', error);
      }
    },
    getPokemonImage(index) {
      // Retorna URL de img Pokémon basada en su índice
      return this.pokemonList[index].image;
    },
    checkPokemon(index) {
      // Verifica si la adivinanza coincide con el nombre del Pokémon
      const currentPokemonName = this.pokemonList[index].name;
      if (this.pokemonList[index].guess.toLowerCase() === currentPokemonName.toLowerCase()) {
        // Si la adivinanza es correcta, se marca como descubierto y se incrementa el contador de adivinanzas correctas
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
  justify-content:space-evenly;
  margin-top: 30px;
  padding: 0 200px;
}

.pokemon-item {
  margin: 10px;
  text-align: center;
}


.counter {
  font-weight: bold;
  text-align: center;
}

.pokemon-item img {
  width: 100px; 
  height: 100px; 
}

.pokemon-details {
  min-height: 100px; 
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.pokemon-item input,
.pokemon-item button,
.pokemon-item p {
  margin: 10px auto;
  width: 80%;
  text-align: center;
}
</style>
