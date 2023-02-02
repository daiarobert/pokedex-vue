<template>
  <div class="logo">
    <img src="/Pokedex.png" class="logo-img" />
  </div>
</template>
<script>
export default {
  data() {
    return {};
  },
  methods: {},
  //invoke on load
  mounted() {},
};
</script>
<script setup>
const props = defineProps({});

$(document).ready(function () {
  //Supplementary JS goes here...
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
.logo-img {
  max-width: 250px;
}
</style>
