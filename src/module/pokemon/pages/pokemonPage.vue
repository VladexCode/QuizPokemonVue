<template>
  <h1 v-if="!pokemon">Espere por favor...</h1>

  <div v-else>
    <h1>¿Quién es este pokémon?</h1>

    <imagenPokemon :pokemon-id="pokemon.id" :show-pokemon="showPokemon" />

    <opcionesPokemon :pokemons="pokemonArr" @selection-pokemon="checkAnswer" />

    <template v-if="showAnswer">
      <h2 class="fade-in">{{ message }}</h2>
      <button @click="newGame">Nuevo Juego</button>
    </template>
  </div>
</template>

<script>
import opcionesPokemon from '@/module/pokemon/components/opcionesPokemon'
import imagenPokemon from '@/module/pokemon/components/imagenPokemon'
import getPokemon from '@/module/pokemon/helpers/getPokemon'
export default {
  components: { opcionesPokemon, imagenPokemon },
  data () {
    return {
      pokemonArr: [],
      pokemon: null,
      showPokemon: false,
      showAnswer: false,
      message: ''
    }
  },
  methods: {
    async mixPokemonArray () {
      this.pokemonArr = await getPokemon()
      const rndInt = Math.floor(Math.random() * 4)
      this.pokemon = this.pokemonArr[rndInt]
    },
    checkAnswer (selectedId) {
      this.showPokemon = true
      this.showAnswer = true
      if (selectedId === this.pokemon.id) {
        this.message = `Correcto, ${this.pokemon.name}`
      } else {
        this.message = `Oops, era ${this.pokemon.name}`
      }
    },
    newGame () {
      this.showPokemon = false
      this.showAnswer = false
      this.pokemonArr = []
      this.pokemon = null
      this.mixPokemonArray()
    }
  },
  mounted () {
    this.mixPokemonArray()
  }
}
</script>
