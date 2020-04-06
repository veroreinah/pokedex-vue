<template>
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
</template>

<script>
export default {
  name: "PokeInput",
  data() {
    return {
      searchText: "",
    };
  },
  methods: {
    setAppliedSearchText() {
      this.$emit("searchByText", this.searchText);
    },
  },
};
</script>

<style lang="scss">
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
  border: 1px solid #ccc;
  padding: 7px 14px 9px;
  transition: 0.4s;

  ~ .focus-border:before,
  ~ .focus-border:after {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 0;
    height: 2px;
    background-color: #4caf50;
    transition: 0.3s;
  }

  ~ .focus-border:after {
    top: auto;
    bottom: 0;
    left: auto;
    right: 0;
  }

  ~ .focus-border i:before,
  ~ .focus-border i:after {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 2px;
    height: 0;
    background-color: #4caf50;
    transition: 0.4s;
  }

  ~ .focus-border i:after {
    left: auto;
    right: 0;
    top: auto;
    bottom: 0;
  }
  
  &:focus ~ .focus-border:before,
  &:focus ~ .focus-border:after {
    width: 100%;
    transition: 0.3s;
  }

  &:focus ~ .focus-border i:before,
  &:focus ~ .focus-border i:after {
    height: 100%;
    transition: 0.4s;
  }
}
</style>
