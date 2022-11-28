<template>
  <NavHeader />
  <p>{{ $store.state.test }}</p>
</template>

<script>
import NavHeader from "../components/NavHeader.vue";

export default {
  name: "App",
  components: {
    NavHeader,
  },
};
</script>

<style>
*,
*::after,
*::before {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html {
  font: 16px Verdana, sans-serif;
}

html,
#app,
body {
  height: 100%;
}
</style>
