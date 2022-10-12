<template>
  <div><LoginForm /></div>
</template>

<script>
import LoginForm from "../components/Auth/Login.vue";
export default {
  name: "IndexPage",
  components: {
    LoginForm,
  },
  layout: "default",
};
</script>
