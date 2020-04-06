<template>
  <div>
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
          <i class="material-icons" :style="{ color: TYPE_COLOR[type] }">
            lens
          </i>
          <span class="label-title">{{ type }}</span>
        </label>
      </div>
    </div>
  </div>
</template>

<script>
import { TYPE_COLOR } from "@/constants";

export default {
  name: "PokeTypeFilter",
  props: ["value"],
  data() {
    return {
      checkedTypes: [],
      TYPE_COLOR,
    };
  },
  computed: {
    sortedTypes() {
      return Object.keys(this.TYPE_COLOR).sort();
    },
  },
  watch: {
    checkedTypes() {
      this.$emit("input", this.checkedTypes);
    },
  },
};
</script>

<style lang="scss">
/* from https://codepen.io/hansmaad/pen/qaGrQL */
.md-checkbox {
  position: relative;
  display: flex;
  align-items: center;
  margin: 16px 0;
  text-align: left;

  &.md-checkbox-inline {
    display: inline-block;
  }
  label {
    display: flex;
    align-items: center;
    cursor: pointer;
    text-transform: capitalize;

    .label-title {
      margin: 0px 5px;
    }

    &:before,
    &:after {
      content: "";
      position: absolute;
      left: 0;
      top: 0;
    }
    &:before {
      width: 20px;
      height: 20px;
      background: #fff;
      border: 2px solid rgba(0, 0, 0, 0.54);
      border-radius: 2px;
      cursor: pointer;
      transition: background 0.3s;
    }
  }
  input[type="checkbox"] {
    outline: 0;
    margin-right: 10px;
    visibility: hidden;
    &:checked + label:before {
      background: #4caf50;
      border: none;
    }
    &:checked + label:after {
      transform: rotate(-45deg);
      top: 5px;
      left: 4px;
      width: 12px;
      height: 6px;
      border: 2px solid #fff;
      border-top-style: none;
      border-right-style: none;
    }
    &:disabled + label:before {
      border-color: rgba(0, 0, 0, 0.26);
    }
    &:disabled:checked + label:before {
      background: rgba(0, 0, 0, 0.26);
    }
  }
}
</style>
