<template>
  <div id="pokemon">
    <el-row>
      <el-card :body-style="{ padding: '0px' }">
        <el-image :src="currentImg"></el-image>
        <!-- <img :src="pokemon.front" class="image"> -->
        <div style="padding: 14px">
          <h1>{{ num }} - {{ name | upper }}</h1>
          <h3>{{ pokemon.type }}</h3>
          <el-button type="primary" round @click="mudarSprite">
            Mudar sprite
          </el-button>
        </div>
      </el-card>
    </el-row>
  </div>
</template>

<script>
import axios from "axios";

export default {
  created: function () {
    axios.get(this.url).then((res) => {
      this.pokemon.type = res.data.types[0].type.name;
      this.pokemon.front = res.data.sprites.front_default;
      this.pokemon.back = res.data.sprites.back_default;
      this.currentImg = this.pokemon.front;
      console.log(this.pokemon);
    });
  },
  data() {
    return {
      isFront: true,
      currentImg: "",
      pokemon: {
        type: "",
        front: "",
        back: "",
      },
    };
  },
  props: {
    num: Number,
    name: String,
    url: String,
  },
  filters: {
    upper: function (value) {
      var newName = value[0].toUpperCase() + value.slice(1);
      return newName;
    },
  },
  methods: {
    mudarSprite: function () {
      if (this.isFront) {
        this.isFront = false;
        this.currentImg = this.pokemon.back;
      } else {
        this.isFront = true;
        this.currentImg = this.pokemon.front;
      }
    },
  },
};
</script>

<style>
#pokemon {
  margin-top: 10%;
}
</style>