<template>
  <v-app>
    <v-container>
        <v-container>
          <v-text-field 
          v-model="search"
          label="Pesquisar"
          placeholder="Charmander"
          solo
          ></v-text-field>

          <v-row>
            <v-col cols=2 v-for="pokemon in filtered_pokemons" :key="pokemon.name">
              <v-card @click="show_pokemon(get_id(pokemon))">
                <v-container>
                  <v-row class="mx-0 d-flex justify-center align-center mt-2">
                    <img width="100%" :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/animated/${get_id(pokemon)}.gif`" :alt="pokemon.name"/>
                  </v-row>
                  <h2 class="text-center mt-3">{{ get_name(pokemon) }}</h2>
                </v-container>
              </v-card>
            </v-col>
          </v-row>
        </v-container>
    </v-container>
    <v-dialog v-model="dialog" max-width="800">
      <v-card v-if="selected_pokemon">
        <v-container>
          <v-row class="d-flex align-center">
            <v-col cols="3">
              <img width="100%" :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/animated/${selected_pokemon.id}.gif`" :alt="selected_pokemon.name"/>
            </v-col>
            <v-col cols="6" class="text-center">
              <h1>{{ get_name(selected_pokemon) }}</h1>
              <v-chip label class="ml-2" v-for="type in selected_pokemon.types" :key="type.slot">
                {{ type.type.name }}
              </v-chip>
              <v-divider class="my-4"></v-divider>
              <v-chip label>
                <span>Altura: {{ selected_pokemon.height * 2.54 }} cm</span>
              </v-chip>
              <v-chip label class="ml-2">
                <span>Peso: {{ (selected_pokemon.weight * 0.453592).toFixed(2) }} kgs</span>
              </v-chip>
            </v-col>
            <v-col cols="3" class="d-flex justify-end">
              <img width="100%" :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/animated/shiny/${selected_pokemon.id}.gif`" :alt="selected_pokemon.name"/>
            </v-col>
            <v-col cols="12" class="d-flex justify-center pt-0">
              <v-chip label>
                <span>Vida: {{ selected_pokemon.stats[0].base_stat  }}</span>
              </v-chip>
              <v-chip label class="ml-2">
                <span>Ataque: {{ selected_pokemon.stats[1].base_stat  }}</span>
              </v-chip>
              <v-chip label class="ml-2">
                <span>Defesa: {{ selected_pokemon.stats[2].base_stat  }}</span>
              </v-chip>
              <v-chip label class="ml-2">
                <span>Velocidade: {{ selected_pokemon.stats[5].base_stat  }}</span>
              </v-chip>
            </v-col>
          </v-row>
          {{ selected_pokemon }}
        </v-container>
      </v-card>
    </v-dialog>

  </v-app>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',

  components: {},

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
    get_id(pokemon){
      return Number(pokemon.url.split("/")[6]);
    },
    get_name(pokemon){
      return pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1);
    },
    show_pokemon(id){
      axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`).then((response) => {
        this.selected_pokemon = response.data;
        this.dialog = !this.dialog;
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
