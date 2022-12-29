<template>
  <NavBar />
  <RouterView />
</template>

<script>
import { RouterView } from 'vue-router';
import NavBar from './components/NavBar.vue';

export default {
  name: "App",
  components: { NavBar, RouterView }
}
</script>

<style lang="scss" scoped>

</style>