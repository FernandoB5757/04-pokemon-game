<template>
  <h1 v-if="!pokemon">Espere por faovor...</h1>
  <div v-else>
    <h1>¿Quien es este pokemon?</h1>
    <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon" />
    <PokemonOption :pokemons="pokemonArr" @selection="checkAnswer" />

    <template v-if="showAnswer">
      <h2 class="fade-in">{{ message }}</h2>
      <button @click="newGame">Nuevo juego</button>
    </template>
  </div>
</template>

<script>
import PokemonOption from "@/components/PokemonOption.vue";
import PokemonPicture from "@/components/PokemonPicture.vue";
import getPokemonOptions from "@/helper/getPokemonOption";

export default {
  components: {
    PokemonOption,
    PokemonPicture,
  },
  data() {
    return {
      pokemonArr: [],
      pokemon: null,
      showPokemon: false,
      showAnswer: false,
      message: "",
    };
  },
  methods: {
    async mixPokemonArray() {
      this.pokemonArr = await getPokemonOptions();

      const rndInt = Math.floor(Math.random() * 4);

      this.pokemon = this.pokemonArr[rndInt];

      //console.log('pokemonArr :>> ', this.pokemonArr);
    },
    checkAnswer(pokemonId) {
      this.showAnswer = true;
      if (pokemonId === this.pokemon.id) {
        this.showPokemon = true;
        this.message = `Correcto, ${this.pokemon.name}`;
      } else {
        this.message = `Oops era, ${this.pokemon.name}`;
      }
    },
    newGame(){
        this.showAnswer = false;
        this.showPokemon = false;
        this.pokemonArr.length = 0;
        this.pokemon = null;
        this.mixPokemonArray();

    },
  },
  mounted() {
    this.mixPokemonArray();
  },
};
</script>
