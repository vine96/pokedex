<template>
<v-card @click="show_pokemon(get_id(pokemon))" :flat="flat">
    <v-container>
      <v-row class="mx-0 d-flex justify-center align-center mt-2">
        <img width="100%" :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/animated/${get_id(pokemon)}.gif`" :alt="pokemon.name"/>
      </v-row>
      <h2 class="text-center mt-3">{{ get_name(pokemon) }}</h2>
    </v-container>
  </v-card>
</template>

<script>
export default{
    props: {
        pokemon: Object,
        flat: {
            type: Boolean,
            default: false
        }
    },
    methods: {
        get_id(pokemon){
            return Number(pokemon.url.split("/")[6]);
        },
        get_name(pokemon){
            return pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1);
        },
        show_pokemon(id){
            this.$emit("clicked", id);
        }
    }
};
</script>

<style lang="scss" scoped></style>