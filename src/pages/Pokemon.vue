<template>
<h1 v-if="!pokemon">Por favor, espere...</h1>
  <div v-else>
      <h1>¿quién es este pokemon?</h1>
      <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon" />
      <PokemonOptions :pokemons="pokemonList" @selection="checkAnswer" />
      
      <div class="fade-in" v-if="showAnswer">
        <h2>{{message}}</h2>
        <button @click="newGame()">Nuevo juego</button>
      </div>

  </div>
</template>

<script>

import PokemonOptions from '@/components/PokemonOptions'
import PokemonPicture from '@/components/PokemonPicture'


import getPokemonOptions from '@/helpers/getPokemonOptions'

export default {
    name:'Pokemon',
    components:{
        PokemonOptions,
        PokemonPicture,
    },
    data(){
        return {
            pokemonList:[],
            pokemon:null,
            showPokemon:false,
            showAnswer:false,
            message:false,
        }
    },
    mounted(){
        this.mixPokemonList()
    },
    methods:{
        async mixPokemonList(){
            
            this.pokemonList = await getPokemonOptions()
            const rndInt = Math.floor(Math.random() * 4)
            this.pokemon = this.pokemonList[rndInt]

        },
        checkAnswer(id){
            
            this.showPokemon = true
            this.showAnswer = true
            this.message = this.pokemon.id===id ? `Correcto, ${this.pokemon.name}` : `Opps era, ${this.pokemon.name}`

        },
        newGame(){

            this.showAnswer = false
            this.showPokemon = false
            this.pokemon = null
            
            this.mixPokemonList()

        }
    }

};
</script>

<style>
</style>