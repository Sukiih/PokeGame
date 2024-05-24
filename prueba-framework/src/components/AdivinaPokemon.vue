<template>
  <div>
    <!-- contador-->
    <span class="contador">Pokemones descubiertos: {{ correctGuesses }}</span>
    <div class="pokemon-game">
      <div v-for="(pokemon, index) in pokemonList" :key="index" class="pokemon-item">
        <!-- filtro desenfoque + grises -->
        <img :src="pokemon.image" :style="{ filter: pokemon.descubierto ? 'none' : 'blur(2px) grayscale(100%)' }" />
        <div class="pokemon-details">
          <!-- input para ingresar name poke -->
          <input v-if="!pokemon.descubierto" type="text" v-model="pokemon.guess" />
          <!-- btn -->
          <button v-if="!pokemon.descubierto" @click="checkPokemon(pokemon, index)">Descubrir</button>
          <!-- nombre pokemon descubierto -->
          <p v-else>{{ pokemon.name }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AdivinaPokemon',
  props: {
    //pt 2
    pokemonList: {
      type: Array,
      required: true
    }
  },
  computed: {
    // pokes descubiertos
    correctGuesses() {
      return this.pokemonList.filter(pokemon => pokemon.descubierto).length;
    }
  },
  methods: {
    checkPokemon(pokemon, index) {
      const currentPokemonName = pokemon.name;
      // Comparar ingresado x usuario con nombre poke
      if (pokemon.guess.toLowerCase() === currentPokemonName.toLowerCase()) {
        // emitiendo indice pokémon descubierto 
        this.$emit('pokemon-descubierto', index);
      } else {
        alert('Nombre incorrecto. ¡Sigue intentando!');
      }
    }
  }
};
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


.contador {
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
