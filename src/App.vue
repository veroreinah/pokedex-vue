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
        <PokeInput @searchByText="searchByText" />

        <!-- Type Filters -->
        <PokeTypeFilter v-model="checkedTypes" />
      </aside>
      <!-- Pokemons View -->
      <main class="main-view">
        <!-- Full Pokemons List in: https://raw.githubusercontent.com/rubnvp/vue-pokedex/master/data/pokemons.json -->
        <article class="pokemons-list">
          <div v-if="!filteredPokemons.length">No results</div>
          <!-- Pokemon Card -->
          <PokemonCard
            v-for="pokemon in filteredPokemons"
            :key="pokemon.id"
            :pokemon="pokemon"
            @removePokemon="removePokemon"
          />
        </article>
      </main>
    </div>
  </div>
</template>

<script>
import PokemonCard from "@/components/PokemonCard";
import PokeInput from "@/components/PokeInput";
import PokeTypeFilter from "@/components/PokeTypeFilter";

export default {
  name: "App",
  data() {
    return {
      appliedSearchText: "",
      checkedTypes: [],
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
    }
  },
  methods: {
    removePokemon(pokemonToRemove) {
      this.pokemons = this.pokemons.filter(
        (pokemon) => pokemon.id !== pokemonToRemove.id
      );
    },
    searchByText(searchText) {
      this.appliedSearchText = searchText;
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
  components: {
    PokemonCard,
    PokeInput,
    PokeTypeFilter
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
</style>
