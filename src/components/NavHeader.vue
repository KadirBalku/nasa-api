<script>
import ApodApi from "./ApodApi.vue";
export default {
  components: {
    ApodApi,
  },
  data() {
    return {
      navTabs: [],
      activeTab: "apodTab",
    };
  },
  methods: {
    tabChanger(event) {
      this.activeTab = event.target.id;
    },
  },
};
</script>

<template>
  <header>
    <h1>NASA</h1>
    <nav class="nav__menu">
      <ul>
        <li>
          <a href="#" @click="tabChanger($event)" id="apodTab">
            Astronomy Picture of the Day
          </a>
        </li>
        <li>Test Section</li>
        <li>About me</li>
        <li>Privacy</li>
        <li>Test Section 2nd</li>
      </ul>
    </nav>
  </header>
  <main>
    <ApodApi v-if="activeTab === 'apodTab'" />
  </main>
</template>

<style scoped>
header {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 1rem;
  gap: 2rem;
  width: 100%;
  min-height: 20%;
  background-color: rgb(27, 27, 27);
  color: rgb(221, 221, 221);
}
main {
  margin: auto;
  max-width: 120ch;
}
h1 {
  font-size: 10rem;
  text-shadow: 2px 2px 5px coral, 6px 3px 28px rgb(255, 255, 255);
}
.nav__menu {
  display: flex;
  background-color: coral;
  justify-content: space-between;
  align-items: center;
}

ul {
  display: flex;
  gap: 3rem;
  padding: 1rem;
}

li {
  cursor: pointer;
}
</style>
