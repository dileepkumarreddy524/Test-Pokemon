<template>
  <div class="container">
    <h1>Pokemon's</h1>

    <ul class="list-group">
      <li class="list-group-item" v-for="(pokemon) in pokemonsList" v-bind:key="pokemon.key">
        <v-layout>
        <img :src="imageURL + pokemon.id + '.png'"/>
        <v-col class="pt-4 pl-5">
        <v-row>
        <span class="font-weight-bold">Name : </span>
        <span>{{pokemon.name}}</span>
        </v-row>
        <v-row>
        <span class="font-weight-bold">Height : </span>
        <span>{{pokemon.height}}</span>
        </v-row>
        <v-row>
        <span class="font-weight-bold">Weight : </span>
        <span>{{pokemon.weight}}</span>
        </v-row>
        </v-col>
        </v-layout>
      </li>
    </ul>
    <v-flex>
        <v-btn  outlined rounded class="ml-5 mt-3" color="primary" @click="load">Load More</v-btn>
    </v-flex>
    <v-btn v-if="api.previous" outlined rounded class="mt-3" color="primary"  @click="previous">Previous</v-btn>
    <v-btn v-if="api.next" outlined rounded class="ml-5 mt-3" color="primary" @click="next">Next</v-btn>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      api: {},
      imageURL:"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/",
      limit: 30,
      pokemonsList: [],
      nextOffset: '',
      previousOffset: ''
    };
  },
  created() {
    this.fetchPokemons();
  },
  methods: {
    async fetchPokemons(url = `https://pokeapi.co/api/v2/pokemon/?limit=${this.limit}`) {
      try {
      let {data} = await axios.get(url)
      this.pokemonsList = []
      this.api = data
      if(this.api.next){
      var nextPaginationURL = new URL(this.api.next)
      this.nextOffset = nextPaginationURL.searchParams.get("offset");
      }
      if(this.api.previous){
      var previousPaginationURL = new URL(this.api.previous)
      this.previousOffset = previousPaginationURL .searchParams.get("offset");
      }
      for (const key of this.api.results) {
      let response = await axios.get(key.url)
      let{height, weight} = response.data
      key.height = height
      key.weight = weight
      key.url = key.url.endsWith('/') ? key.url.slice(0, -1) : key.url
      key.id = key.url.substring(key.url.lastIndexOf('/')).replace('/','');
      this.pokemonsList.push(key)
      }
      }
      catch(e){
        console.log(e)
      }
    },
    next() {
      this.fetchPokemons(`https://pokeapi.co/api/v2/pokemon/?offset=${this.nextOffset}&limit=30`);
    },
    previous() {
      this.fetchPokemons(`https://pokeapi.co/api/v2/pokemon/?offset=${this.previousOffset}&limit=30`);
    },
    load() {
      this.fetchPokemons(`https://pokeapi.co/api/v2/pokemon/?offset=${this.nextOffset}&limit=15`);
    }
  }
};
</script>