<template>
  <h1 v-if="!pokemon"> Espere por favor...</h1>
  <div v-else>
    <h1> Quien es este pokemon?</h1>
    <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon"/>
    <PokemonOptions :pokemons="pokemonArr" @selection="checkAnswer" />
  </div>
  <div v-if="showAnswer" class="game-options">
      <h2> {{ message }}</h2>
      <h4> Tu puntuacion: {{ score }}</h4>
     <button @click="NewGame"> Nuevo juego</button>
  </div>


  <!-- TODO: Picture-->

  <!-- TODO:Opciones-->
</template>

<script>
import PokemonOptions from '@/components/PokemonOptions.vue'
import PokemonPicture from '@/components/PokemonPicture.vue'

import getPokemonOptions from '@/helpers/getPokemonOptions.js'

console.log(getPokemonOptions())

export default {
  components: {
    PokemonOptions,
    PokemonPicture
  },
  data() {
    return {
      pokemonArr: [],
      pokemon:null,
      showPokemon:false,
      showAnswer:false,
      message:'',
      score:0
    }
  },
  methods: {
    async mixPokemonArray(){
      this.pokemonArr = await getPokemonOptions()
      const rndInt = Math.floor(Math.random()*4)
      this.pokemon = this.pokemonArr[rndInt]
    },
    checkAnswer(pokemonId){
      this.showPokemon = true
      this.showAnswer = true
       if (pokemonId === this.pokemon.id){
         this.message= `Es ${this.pokemon.name}`
         this.score++
       }else{
        this.message =`Oops, era ${this.pokemon.name}`
       }
    },
    NewGame(){
        this.mixPokemonArray()
        this.showPokemon = false
        this.showAnswer = false
        this.pokemonArr = []
        this.pokemon = null
        this.message =''
    }
  },
  mounted() {
     this.mixPokemonArray()
  }
}
</script>

<style>

.game-options{
   display:flex;
   align-items: center;
   justify-content: center;
   flex-direction: column;
}


</style>