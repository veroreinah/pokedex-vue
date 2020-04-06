<template>
  <div id="app">
    <!-- Header -->
    <header class="main-header">
      Vue - Pokedex
    </header>
    <!-- Content -->
    <div id="app" class="main-content" v-cloak>
      <!-- Left Side Filters -->
      <aside class="pokedex-filters">
        <!-- Searchbox -->
        <div class="nice-input-wrapper">
          <input
            v-model="searchText"
            @keyup.enter="setAppliedSearchText"
            type="text"
            placeholder="Search by name"
            class="nice-input"
          />
          <span class="focus-border"><i></i></span>
        </div>
        <!-- Type Filters -->
        <p class="checkboxes-list-title">Type</p>
        <div class="checkboxes-list">
          <!-- Type Selector -->
          <div v-for="type in sortedTypes" :key="type" class="md-checkbox">
            <input
              :id="`checkbox-${type}`"
              type="checkbox"
              :value="type"
              v-model="checkedTypes"
            />
            <label :for="`checkbox-${type}`">
              <i class="material-icons" :style="{ color: TYPE_COLOR[type] }"
                >lens</i
              >
              <span class="label-title">{{ type }}</span>
            </label>
          </div>
        </div>
      </aside>
      <!-- Pokemons View -->
      <main class="main-view">
        <!-- Full Pokemons List in: https://raw.githubusercontent.com/rubnvp/vue-pokedex/master/data/pokemons.json -->
        <article class="pokemons-list">
          <div v-if="!filteredPokemons.length">No results</div>
          <!-- Pokemon Card -->
          <div
            v-for="pokemon in filteredPokemons"
            :key="pokemon.id"
            class="pokemon-card"
          >
            <div class="background-wrapper">
              <div
                v-for="type in pokemon.types"
                :key="type"
                :style="{ 'background-color': TYPE_COLOR[type] }"
              ></div>
            </div>
            <span class="remove-pokemon" @click="removePokemon(pokemon)">
              <i class="material-icons">close</i>
            </span>
            <div class="image-wrapper">
              <img
                :src="pokemon.image"
                :alt="pokemon.name"
                class="pokemon-image"
              />
            </div>
            <div class="pokemon-title">
              {{ pokemon.name | mimi }}
            </div>
          </div>
        </article>
      </main>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      searchText: "",
      appliedSearchText: "",
      checkedTypes: [],
      TYPE_COLOR: {
        grass: "#78C850",
        poison: "#A040A0",
        fire: "#F08030",
        flying: "#A890F0",
        water: "#6890F0",
        bug: "#A8B820",
        normal: "#A8A878",
        electric: "#F8D030",
      },
      pokemons: [],
    };
  },
  computed: {
    filteredPokemons() {
      const pokemonsByName = this.pokemons.filter((pokemon) =>
        pokemon.name.includes(this.appliedSearchText)
      );

      // AND search in types
      let pokemonsByType = pokemonsByName;
      this.checkedTypes.forEach((type) => {
        pokemonsByType = pokemonsByType.filter((pokemon) =>
          pokemon.types.includes(type)
        );
      });

      // OR search in types
      // const pokemonsByType = pokemonsByName.filter(
      //   pokemon => !this.checkedTypes.length || pokemon.types.some(type => this.checkedTypes.includes(type)));

      return pokemonsByType;
    },
    sortedTypes() {
      return Object.keys(this.TYPE_COLOR).sort();
    },
  },
  methods: {
    removePokemon(pokemonToRemove) {
      this.pokemons = this.pokemons.filter(
        (pokemon) => pokemon.id !== pokemonToRemove.id
      );
    },
    setAppliedSearchText() {
      this.appliedSearchText = this.searchText;
    },
  },
  created() {
    fetch(
      "https://raw.githubusercontent.com/rubnvp/vue-pokedex/master/data/pokemons.json"
    )
      .then((response) => response.json())
      .then((pokemons) => (this.pokemons = pokemons))
      .catch((error) => console.error(error));
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css?family=Roboto:400,700");
@import url("https://fonts.googleapis.com/icon?family=Material+Icons");

*,
*:before,
*:after {
  box-sizing: border-box;
}

:focus {
  outline: none;
}

.material-icons {
  font-size: 20px;
}

body {
  font-family: "Roboto", sans-serif;
  display: flex;
  min-height: 100vh;
  flex-direction: column;
  margin: 0;
}

header,
article,
nav,
aside {
  padding: 1em;
}

.main-header {
  text-transform: uppercase;
  background: #42b883;
  color: white;
  font-size: 35px;
  padding: 0.5em;
  font-weight: 400;
}

.main-content {
  display: flex;
  flex: 1;
}

.main-view {
  overflow-y: auto;
}

.pokedex-filters {
  flex: 0 0 250px;
  display: flex;
  flex-direction: column;
  box-shadow: 0 2px 2px 0 rgba(0, 0, 0, 0.14), 0 1px 5px 0 rgba(0, 0, 0, 0.12),
    0 3px 1px -2px rgba(0, 0, 0, 0.2);
}

.checkboxes-list-title {
  margin-bottom: 0px;
}

.checkboxes-list {
  overflow-y: auto;
}

.pokemons-list {
  flex: 1;
  display: flex;
  flex-wrap: wrap;
  overflow-y: auto;
}

/* Pokemon card */
.pokemon-card {
  margin: 5px;
  position: relative;
  height: 170px;
  width: 170px;
  display: flex;
  flex-direction: column;
  text-align: center;
  color: white;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
  transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
  overflow: hidden;
}

.pokemon-card:hover {
  box-shadow: 0 14px 28px rgba(0, 0, 0, 0.25), 0 10px 10px rgba(0, 0, 0, 0.22);
}

.pokemon-card .background-wrapper {
  position: absolute;
  width: 100%;
  height: 100%;
  display: flex;
  z-index: -1;
}

.pokemon-card .background-wrapper div {
  flex-grow: 1;
}

.pokemon-card .remove-pokemon {
  display: none;
  position: absolute;
  top: 0;
  right: 0;
  cursor: pointer;
}

.pokemon-card:hover .remove-pokemon {
  display: inline-block;
}

.pokemon-card .remove-pokemon:hover {
  background-color: rgba(255, 255, 255, 0.2);
}

.pokemon-card .image-wrapper {
  flex-grow: 1;
  display: flex;
  justify-content: center;
  align-items: center;
}

.pokemon-card .pokemon-image {
  height: 110px;
}

.pokemon-card .pokemon-title {
  height: 40px;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  text-transform: capitalize;
}
/* End Pokemon card */

/* Nice input */
/* from https://codepen.io/Takumari85/pen/RaYwpJ */
.nice-input-wrapper {
  position: relative;
}
.nice-input {
  font: 15px/24px "Muli", sans-serif;
  color: #333;
  width: 100%;
  box-sizing: border-box;
  letter-spacing: 1px;
}
.nice-input {
  border: 1px solid #ccc;
  padding: 7px 14px 9px;
  transition: 0.4s;
}
.nice-input ~ .focus-border:before,
.nice-input ~ .focus-border:after {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 0;
  height: 2px;
  background-color: #4caf50;
  transition: 0.3s;
}
.nice-input ~ .focus-border:after {
  top: auto;
  bottom: 0;
  left: auto;
  right: 0;
}
.nice-input ~ .focus-border i:before,
.nice-input ~ .focus-border i:after {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 2px;
  height: 0;
  background-color: #4caf50;
  transition: 0.4s;
}
.nice-input ~ .focus-border i:after {
  left: auto;
  right: 0;
  top: auto;
  bottom: 0;
}
.nice-input:focus ~ .focus-border:before,
.nice-input:focus ~ .focus-border:after {
  width: 100%;
  transition: 0.3s;
}
.nice-input:focus ~ .focus-border i:before,
.nice-input:focus ~ .focus-border i:after {
  height: 100%;
  transition: 0.4s;
}
/* End Nice input */

/* Nice checkbox */
/* from https://codepen.io/hansmaad/pen/qaGrQL */
.md-checkbox {
  position: relative;
  display: flex;
  align-items: center;
  margin: 16px 0;
  text-align: left;
}
.md-checkbox.md-checkbox-inline {
  display: inline-block;
}
.md-checkbox label {
  display: flex;
  align-items: center;
  cursor: pointer;
  text-transform: capitalize;
}

.md-checkbox label .label-title {
  margin: 0px 5px;
}

.md-checkbox label:before,
.md-checkbox label:after {
  content: "";
  position: absolute;
  left: 0;
  top: 0;
}
.md-checkbox label:before {
  width: 20px;
  height: 20px;
  background: #fff;
  border: 2px solid rgba(0, 0, 0, 0.54);
  border-radius: 2px;
  cursor: pointer;
  transition: background 0.3s;
}
.md-checkbox input[type="checkbox"] {
  outline: 0;
  margin-right: 10px;
  visibility: hidden;
}
.md-checkbox input[type="checkbox"]:checked + label:before {
  background: #4caf50;
  border: none;
}
.md-checkbox input[type="checkbox"]:checked + label:after {
  transform: rotate(-45deg);
  top: 5px;
  left: 4px;
  width: 12px;
  height: 6px;
  border: 2px solid #fff;
  border-top-style: none;
  border-right-style: none;
}
.md-checkbox input[type="checkbox"]:disabled + label:before {
  border-color: rgba(0, 0, 0, 0.26);
}
.md-checkbox input[type="checkbox"]:disabled:checked + label:before {
  background: rgba(0, 0, 0, 0.26);
}
/* End Nice checkbox */
</style>
