<script>
  import { RouterLink, RouterView } from "vue-router";

  import NavBar from "./components/NavBar.vue";

  export default {
    components: {
      NavBar,
    },
  };
</script>

<template>
  <NavBar />
  <RouterView style="margin-top: 6rem" />
</template>
