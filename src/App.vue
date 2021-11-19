<template>
  <div id="app">
    <fig-select v-model="selected">
      <option value="1">Item 1</option>
      <option value="2">Item 2</option>
      <option value="3">Item 3</option>
    </fig-select>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import FigSelect from "./components/FigSelect.vue";

export default Vue.extend({
  name: "App",
  components: {
    FigSelect,
  },
  data() {
    return {
      selected: "",
    };
  },
});
</script>

<style lang="scss">
body {
  padding: 1rem;
}
</style>
