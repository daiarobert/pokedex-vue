<template>
  <div class="modal-overlay" v-if="props.showModal">
    <div class="modal-content d-flex">
      <img src="/arrow-back.svg" class="close-modal" @click="closeModal()" />

      <div class="left col-12 col-md-7">
        <div
          class="pokemon-card d-flex flex-column justify-content-center align-items-center"
        >
          <div
            class="top-card d-flex flex-column justify-content-center align-items-center"
            :style="
              props.pokemonType == 'fire'
                ? 'background-color: #FDDFDF'
                : props.pokemonType == 'grass'
                ? 'background-color: #DEFDE0'
                : props.pokemonType == 'electric'
                ? 'background-color: #FCF7DE'
                : props.pokemonType == 'water'
                ? 'background-color: #f4e7da'
                : props.pokemonType == 'ground'
                ? 'background-color: #f4e7da'
                : props.pokemonType == 'rock'
                ? 'background-color: #d5d5d4'
                : props.pokemonType == 'fairy'
                ? 'background-color: #fceaff'
                : props.pokemonType == 'poison'
                ? 'background-color: #98d7a5'
                : props.pokemonType == 'bug'
                ? 'background-color: #f8d5a3'
                : props.pokemonType == 'dragon'
                ? 'background-color: #97b3e6'
                : props.pokemonType == 'psychic'
                ? 'background-color: #eaeda1'
                : props.pokemonType == 'flying'
                ? 'background-color: #F5F5F5'
                : props.pokemonType == 'fighting'
                ? 'background-color: #E6E0D4'
                : 'background-color: #e2dcdc'
            "
          >
            <span class="title">{{
              props.pokemon[props.activePokemon].name
            }}</span>
            <img
              :src="props.pokemon[props.activePokemon].img"
              class="pokemon-image-modal"
            />
          </div>
          <span class="special-ability">special ability:</span>
          <div class="pokemon-abilities d-flex">
            <div
              class="ability"
              v-for="(item, key) in props.pokemon[props.activePokemon]
                .abilities"
              :key="key"
            >
              {{ item.ability.name }}
            </div>
          </div>
          <span class="stats-title">stats:</span>
          <div
            class="pokemon-stats-wrapper row d-flex align-items-start justify-content-center w-100;"
          >
            <div
              class="stats-wrapper col-5"
              v-for="(item, key) in props.pokemon[props.activePokemon].stats"
              :key="key"
            >
              <div class="stats d-flex flex-column align-items-start w-100">
                {{ item.stat.name }}:
                {{ item.base_stat }}
              </div>
              <div class="progress" style="height: 13px">
                <div
                  class="progress-bar"
                  role="progressbar"
                  :style="`min-width: ${item.base_stat}%; max-width:160px`"
                  aria-valuemin="0"
                  aria-valuemax="160"
                ></div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="right col-12 col-md-5">
        <div class="container">
          <div class="menu">
            <button class="toggle">
              {{ props.pokemon[props.activePokemon].name }}
            </button>
            <ul class="list">
              <li
                class="list-item"
                style="--delay: 0.2s"
                v-for="(item, key) in props.pokemon"
                :key="key"
                @click="
                  getActivePokemon(key);
                  getType(item.type[0]);
                "
              >
                {{ item.name }}
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  activePokemon: {
    type: Number,
    default: 1,
  },
  pokemonType: {
    type: String,
    default: "",
  },
  pokemon: {},
  showModal: {
    type: Boolean,
    default: false,
  },
});
</script>
<script>
export default {
  data() {
    return {};
  },
  methods: {
    closeModal() {
      this.$emit("closeModal", false);
    },
    getActivePokemon(data) {
      this.$emit("newActivePokemon", data);
    },
    getType(data) {
      this.$emit("newPokemonType", data);
      console.log(data);
    },

    getId(index) {
      this.$emit("activePokemon", index);
      this.showModal = true;
      this.activePokemon = index;
    },
  },

  //invoke on load
  mounted() {},
};
</script>
<style>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgb(0, 0, 0);
  z-index: 999;
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: #2c3e50;
  border-radius: 4px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.235);
  padding: 20px;
  height: 70%;
  width: 55% !important;
  background-color: #f1f1f1 !important;
  border-radius: 25px !important;
  flex-direction: row !important;
}
.left {
  align-items: center;
  justify-content: center;
  display: flex;
}
.right {
  display: flex;
  justify-content: center;
  align-items: center;
}
.pokemon-card {
  margin-right: 10px;
  background-color: white;
  border-radius: 25px;
}
.pokemon-image-modal {
  max-width: 300px;
  min-width: 300px;
  max-height: 200px;
  min-height: 200px;
  position: relative;
  bottom: -25px;
}
.pokemon-abilities {
  margin-top: 15px;
  width: 100%;
  justify-content: space-evenly;
}
.pokemon-stats-wrapper {
  margin-top: 15px;
  margin-bottom: 50px;
}

.top-card {
  width: 100%;
  border-radius: 25px;
}
.ability {
  border: 1px solid;
  border-radius: 25px;
  padding: 5px;
  min-width: 110px;
  max-width: 130px;
  /* font-size: 10px; */

  /* font-family: termina, sans-serif;
  text-transform: uppercase; */
  /* text-transform: uppercase; */
  background-color: black;
  color: white;
}
.special-ability {
  font-family: termina, sans-serif;
  text-transform: uppercase;
  font-weight: 500;
  font-size: 16px;
  width: 100%;
  margin-top: 35px;
  text-align: left;
  margin-left: 50px;
}
.stats-title {
  font-family: termina, sans-serif;
  text-transform: uppercase;
  font-weight: 500;
  font-size: 16px;
  width: 100%;
  margin-top: 15px;
  text-align: left;
  margin-left: 50px;
}
.progress-bar {
  background-color: rgb(62, 54, 54) !important;
}
.close-modal {
  position: absolute;
  width: 35px;
  top: 10px;
  left: 10px;
  cursor: pointer;
}
.container {
  width: 110%;
}
.menu {
  display: grid;
  /* grid-template-rows: 40px max-content; */
  gap: 10px;
}

.toggle {
  all: unset;
  font-family: termina, sans-serif;
  text-transform: uppercase;
  background-color: black;
  color: white;
  width: 100%;
  padding: 10px;
  font-weight: 500;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-radius: 5px;
  cursor: pointer;
}

.title {
  font-family: termina, sans-serif;
  text-transform: uppercase;
  font-weight: 500;
  margin-bottom: 20px;
  margin-top: 20px;
  font-size: 20px;
  position: relative;
  bottom: -15px;
}

.list {
  font-family: termina, sans-serif;
  text-transform: uppercase;
  background-color: white;
  color: #444;
  list-style: none;
  display: grid;
  grid-template-rows: repeat(40px);
  border-radius: 5px;

  height: 500px;
  transition: 0.4s;
  overflow: scroll;
}

.list-item {
  display: flex;
  align-items: center;
  padding-left: 10px;
  cursor: pointer;
  height: 40px;
  border-bottom: 1px solid black;
}

.list-item:hover {
  background-color: black;
  color: #fff;
}

@media screen and (max-width: 768px) {
  .modal-content {
    flex-direction: column !important;
    height: 90% !important;
    width: 100% !important;
    padding: 0 !important;
  }
  .list {
    max-height: 340px;
  }
  .right {
    margin-top: 15px !important;
  }
  .pokemon-card {
    margin-right: 0 !important;
  }
  .top-card img {
    max-width: 200px !important;
    min-width: 200px !important;
    max-height: 150px !important;
    min-height: 150px !important;
  }
  .stats {
    font-size: 12px;
  }
  .pokemon-stats-wrapper {
    margin-bottom: 15px !important;
  }
  .container {
    width: 100% !important;
  }
  .list-item {
    height: 25px !important;
  }
  .title {
    margin-bottom: 0px !important;
    margin-top: 5px !important;
  }
}
@media screen and (max-width: 420px) {
  .list {
    max-height: 270px;
  }
}
@media screen and (max-width: 400px) {
  .list {
    max-height: 215px;
  }
}
@media screen and (max-width: 376px) {
  .list {
    max-height: 65px;
  }
}
</style>
