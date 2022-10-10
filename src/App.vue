<template>
  <v-app>
    <v-container>
        <v-container>
          <v-row>
            <v-container>
              <v-img
                :src="require('../src/assets/pokedex.png')"
                class="my-3"
                contain
                height="200"
              />
              <h1 class="text-center white--text mb-2" style="font-size: 5rem">
                Pokedex
              </h1>
              <h1 class="text-center white--text mb-8">
                Created by
                <a class="red--text" href="https://github.com/vine96">Vine96</a>
              </h1>
            </v-container>
          </v-row>

          <v-text-field 
          v-model="search"
          label="Pesquisar"
          placeholder="Charmander"
          solo
          ></v-text-field>

          <v-row>
            <v-col cols=2 v-for="pokemon in filtered_pokemons" :key="pokemon.name">
              <PokemonCard :pokemon="pokemon" @clicked="show_pokemon" />
            </v-col>
          </v-row>
        </v-container>
    </v-container>

  </v-app>
</template>

<script>
import axios from 'axios';
import PokemonCard from './components/PokemonCard.vue';

export default {
  name: 'App',

  components: { PokemonCard },

  data() {
    return {
      pokemons: [],
      search: "",
      dialog: false,
      selected_pokemon: null
    }
  },

  mounted() {
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151").then((response) => {
      this.pokemons = response.data.results;
    });
  },

  methods: {
    get_move_level(move){
      for(let version of move.version_group_details){
        if(version.version_group.name == "sword-shield" && version.move_learn_method.name == "level-up"){
          return version.level_learned_at;
        }
      }
      return 0;
    },
    filter_moves(pokemon){
      return pokemon.moves.filter((item) => {
        let include = false;
        for(let version of item.version_group_details){
          if(version.version_group.name == "sword-shield" && version.move_learn_method.name == "level-up"){
            include = true;
          }
        }
        return include;
      });
    }
  },

  computed: {
    filtered_pokemons(){
      return this.pokemons.filter((item) => {
        return item.name.includes(this.search);
      });
    }
  }
};
</script>

<style>
#app {
  background: linear-gradient(
      to bottom right,
      rgba(10, 10, 10, 1),
      rgba(12, 39, 63, 1)
    )
    no-repeat center center fixed !important;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover !important;
  background-position: center;
  min-height: 100vh;
}
img {
  max-width:80px;
  max-height:80px;
  min-width:80px;
  min-height:80px;
}
.container{
  min-width: 170px;
  min-height: 125px;
}
</style>
