<template>
  <div class="intro row d-flex" id="pokemon" v-if="pokemon">
    <div
      v-for="(item, key) in pokemon"
      @click="getId(key), getPokemonType(pokemon[key].type[0])"
      :key="item"
      :class="`pokemon-wrapper col-6 col-md-4 d-flex justify-content-center pokemon-modal`"
      :style="
        pokemon[key].type[0] == 'fire'
          ? 'background-color: #FDDFDF'
          : pokemon[key].type[0] == 'grass'
          ? 'background-color: #DEFDE0'
          : pokemon[key].type[0] == 'electric'
          ? 'background-color: #FCF7DE'
          : pokemon[key].type[0] == 'water'
          ? 'background-color: #f4e7da'
          : pokemon[key].type[0] == 'ground'
          ? 'background-color: #f4e7da'
          : pokemon[key].type[0] == 'rock'
          ? 'background-color: #d5d5d4'
          : pokemon[key].type[0] == 'fairy'
          ? 'background-color: #fceaff'
          : pokemon[key].type[0] == 'poison'
          ? 'background-color: #98d7a5'
          : pokemon[key].type[0] == 'bug'
          ? 'background-color: #f8d5a3'
          : pokemon[key].type[0] == 'dragon'
          ? 'background-color: #97b3e6'
          : pokemon[key].type[0] == 'psychic'
          ? 'background-color: #eaeda1'
          : pokemon[key].type[0] == 'flying'
          ? 'background-color: #b6e3d5'
          : pokemon[key].type[0] == 'fighting'
          ? 'background-color: #E6E0D4'
          : 'background-color: #e2dcdc'
      "
      :data-id="`${key}`"
    >
      <div class="card-pokemon">
        <img :src="`${item.img}`" :class="`pokemon-img`" />
        <div class="pokemon-name">
          {{ item.name }}
        </div>
      </div>
    </div>

    <ModalComponent
      :showModal="showModal"
      :activePokemon="activePokemon"
      :pokemon="pokemon"
      :pokemonType="pokemonType"
      @closeModal="getCloseModal($event)"
      @newActivePokemon="getNewActivePokemon($event)"
      @newPokemonType="getNewPokemonType($event)"
    />
  </div>

  <div v-else><img src="/pokeBall.png" class="loader" /></div>
</template>
<script>
export default {
  data() {
    return {
      pokemon: null,
      showModal: false,
      activePokemon: 1,
      pokemonType: "",
    };
  },
  methods: {
    getCloseModal(data) {
      this.showModal = data;
      console.log("din item: " + this.showModal);
    },
    getNewActivePokemon(data) {
      this.activePokemon = data;
    },
    getNewPokemonType(data) {
      this.pokemonType = data;
    },
    getPokemonType(data) {
      this.pokemonType = data;
    },
    created() {
      const promises = [];
      for (let i = 1; i <= 150; i++) {
        const url = `https://pokeapi.co/api/v2/pokemon/${i}`;
        promises.push(fetch(url).then((res) => res.json()));
      }
      Promise.all(promises).then((results) => {
        var pokemons = {};
        results.forEach((data, i) => {
          pokemons[i + 1] = {
            name: data.name,
            id: data.id,
            img: data.sprites.other.dream_world["front_default"],
            type: data.types.map((type) => type.type.name).slice(0, 1),
            stats: data.stats,
            abilities: data.abilities,
          };
        });
        this.pokemon = JSON.parse(JSON.stringify(pokemons));
      });
    },

    getId(index) {
      this.$emit("activePokemon", index);
      this.showModal = true;
      this.activePokemon = index;
    },
  },

  //invoke on load
  mounted() {
    this.created();
  },
};
</script>
<script setup>
import gsap from "gsap";
import ModalComponent from "./modal.vue";
import { CSSRulePlugin } from "gsap/CSSRulePlugin";
import { ScrollTrigger } from "gsap/ScrollTrigger";
gsap.registerPlugin(CSSRulePlugin, ScrollTrigger);
const props = defineProps({
  brandExperience: {
    type: String,
    default: "1",
  },
});

$(document).ready(function () {
  //Supplementary JS goes here...
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
.intro {
  justify-content: space-evenly;
  max-width: 950px;
  justify-content: center;
  margin-left: auto;
  margin-right: auto;
  margin-top: 100px;
}
.pokemon-img {
  max-width: 150px;
  position: relative;
  top: -90px;
  max-height: 130px;
  min-height: 130px;
}
.pokemon-wrapper {
  display: flex;
  justify-content: center;
  color: white;
  border-radius: 25px;
  padding: 30px;
  max-width: 250px;
  max-height: 150px;
  margin: 25px;
  margin-bottom: 55px;
  transition: 0.1s ease-in;
  cursor: pointer;
}
.pokemon-wrapper:hover {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.8), 0 0px 1px 0 rgba(0, 0, 0, 0.19);
}
.pokemon-name {
  font-weight: 500;
  text-decoration: none !important;
  color: black;
  font-family: termina, sans-serif;
  text-transform: uppercase;
  position: relative;
  bottom: 65px;
}
.loader {
  max-width: 300px;
  display: block;
  margin: auto;
  margin-top: 200px;
}
@media screen and (max-width: 600px) {
  .pokemon-wrapper {
    max-width: 170px;
  }
  .loader {
    max-width: 200px;
    margin-top: 100px;
  }
}
@media screen and (max-width: 450px) {
  .card-pokemon img {
    max-width: 90px !important;
    position: relative !important;
    top: -90px !important;
    max-height: 80px !important;
    min-height: 80px !important;
  }
  .pokemon-wrapper {
    max-width: 130px;
    max-height: 85px;
  }
  .pokemon-name {
    bottom: 70px !important;
    font-size: 13px !important;
  }
}
</style>
