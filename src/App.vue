<template>
  <div id="app">
    <el-row type="flex" class="row-bg" justify="center">
      <el-col :span="8">
        <img id="logo" src="./assets/logo-webird-1.png" alt="logo" />
        <hr />
        <h1>Pokedex</h1>
        <el-input
          placeholder="Buscar Pokemon pelo nome em minúsculo"
          v-model="busca"
          clearable
        >
        </el-input>
        <el-button id="btn-busca" type="info" @click="buscar">Procurar</el-button>
        <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
          <Pokemon :name="poke.name" :url="poke.url" :num="index + 1" />
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import axios from "axios";
import Pokemon from "./components/Pokemon";

export default {
  name: "App",
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: "",
    };
  },
  created: function () {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((res) => {
        this.pokemons = res.data.results;
        this.filteredPokemons = res.data.results;
        console.log("pegou lista pokemons");
      });
  },
  components: {
    Pokemon,
  },
  methods: {
    buscar: function () {
      this.filteredPokemons = this.pokemons;
      if (this.busca == "" || this.busca == " ") {
        this.filteredPokemons = this.pokemons;
      } else {
        this.filteredPokemons = this.pokemons.filter((pokemon) => pokemon.name == this.busca);
      }
    },
  },
  // computed: {
  //   resultadoBusca: function () {
  //     if (this.busca == "" || this.busca == " ") {
  //       return this.pokemons;
  //     } else {
  //       return this.pokemons.filter((pokemon) => pokemon.name == this.busca);
  //     }
  //   },
  // },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#logo {
  border-radius: 1rem;
  width: 50%;
  background-color: #0424b5;
  padding: 1rem;
}
#btn-busca {
  margin-top: 1rem;
}
</style>
