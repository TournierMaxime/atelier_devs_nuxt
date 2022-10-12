<template>
  <div>
    <NavBar />
    <Nuxt />
    <FooterBar />
  </div>
</template>

<script>
import NavBar from "../components/Base/Nav.vue";
import FooterBar from "../components/Base/Footer.vue";
export default {
  name: "DefaultLayout",
  components: {
    NavBar,
    FooterBar,
  },
};
</script>
