<template>
  <Menubar class="flex" :model="items">
    <template #start>{{ label }}</template>
    <template v-if="isLogged === true" #end>
      <Button
        label="Admin"
        class="p-button-sm p-button-outlined p-button-info p-menubar-end-spacing"
      />
      &nbsp;
      <Button
        label="Logout"
        class="p-button-sm p-button-outlined p-button-danger p-menubar-end-spacing"
      />
    </template>
  </Menubar>
</template>

<script>
import Menubar from "primevue/menubar";
import Button from "primevue/button";
export default {
  name: "NavBar",
  components: {
    Menubar,
    Button,
  },
  data() {
    return {
      isLogged: false,
      label: "",
      items: [
        {
          label: "Home",
          icon: "pi pi-home",
          url: "/",
        },
        {
          label: "Info",
          url: "/info",
        },
      ],
    };
  },
  methods: {},
};
</script>
