<template>
    <v-dialog v-model="show" max-width="800">
        <v-card v-if="selected_pokemon" class="px-4">
          <v-container>
            <v-row class="d-flex align-center">
              <v-col cols="3">
                <img width="100%" :alt="selected_pokemon.name" 
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/animated/${selected_pokemon.id}.gif`" 
                />
              </v-col>
              <v-col cols="6" class="text-center">
                <h1>{{ get_name(selected_pokemon) }}</h1>
                
                <PokemonType v-for="type in selected_pokemon.types"
                :key="type.slot" :type="type.type.name"/>

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
              <!-- <v-col cols="12" class="d-flex justify-center pt-0">
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
              </v-col> -->
            </v-row>

            <h2 class="mt-4">Stats</h2>
            <Stats class="mt-2" :pokemon="selected_pokemon" />

            <h2>Moves</h2>
            <v-expansion-panels class="mb-4">
                <v-expansion-panel v-for="move_type in ['level-up', 'egg', 'machine', 'tutor']"
                :key="move_type">
                </v-expansion-panel>
                <v-expansion-panel-header>
                    {{ move_type }}
                </v-expansion-panel-header>
                <v-expansion-panel-content>
                    <v-simple-table>
                        <template>
                            <thead>
                                <tr>
                                    <th class="text-left">Level</th>
                                    <th class="text-left">Method</th>
                                    <th class="text-left">Name</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="move in moves[move_type]" :key="move.name">
                                    <td>
                                        <span v-show="move.level != 0">{{ move.level }}</span>
                                    </td>
                                    <td><MoveMethodImage :method="move.method"/></td>
                                    <td>{{ move.name }}</td>
                                </tr>
                            </tbody>
                        </template>
                    </v-simple-table>
                </v-expansion-panel-content>
            </v-expansion-panels>
            <!-- <v-simple-table>
              <template v-slot:default>
                <thead>
                  <tr>
                    <th class="text-left">Level</th>
                    <th class="text-left">Name</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="item in filter_moves(selected_pokemon)" :key="item.move.name">
                    <td>{{ get_move_level(item) }}</td>
                    <td>{{ item.move.name }}</td>
                  </tr>
                </tbody>
              </template>
            </v-simple-table> -->

            <h2>Evolução</h2>
            
          </v-container>
        </v-card>
      </v-dialog>
</template>

<script>
import PokemonType from "./PokemonType.vue";
import Stats from "./Stats.vue";
import MoveMethodImage from "./MoveMethodImage.vue";

export default{
    components:{
        PokemonType,
        Stats,
        MoveMethodImage
    },
    props:{
        show: Boolean,
        selected_pokemon: Object
    },
    methods:{
        get_name(pokemon){
            return pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1);
        }
    }
}
</script>

<style lang="scss" scoped></style>