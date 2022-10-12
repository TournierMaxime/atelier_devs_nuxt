<template>
  <div
    class="flex justify-content-center login m-auto xl:col-4 col-offset-4 lg:col-6 col-offset-3 md:col-8 col-offset-2 sm:col-10 col-offset-1"
  >
    <div class="card">
      <Divider />
      <form class="p-fluid">
        <div class="field">
          <span class="p-float-label p-input-icon-right">
            <i class="pi pi-envelope" />
            <InputText
              v-model="email"
              name="email"
              type="email"
              onChange="email"
              id="email"
            />
            <label for="email">Email*</label>
          </span>
        </div>

        <div class="field">
          <span class="p-float-label">
            <Password
              v-model="password"
              name="password"
              onChange="password"
              toggleMask
              type="password"
              :feedback="false"
              id="password"
            />
            <label for="password">Password*</label>
          </span>
        </div>

        <div class="field text-center">
          <a @click="toggleSignUp">Inscription</a>
          <div v-if="signUpDialog">
            <Dialog
              class="dialog"
              header="Formulaire d'inscription"
              modal
              :visible="signUpDialog"
              v-model="signUpDialog"
              :closable="false"
            >
              <Button
                class="p-button-text"
                @click="closeSignUp"
                label="Fermer"
                icon="pi pi-times"
              />
            </Dialog>
          </div>
        </div>
        <div class="field text-center">
          <a @click="toggleResetPassword"> Mot de passe oublié ? </a>
          <div v-if="resetPasswordDialog">
            <Dialog
              class="dialog"
              header="Formulaire de récupération de mot de passe"
              modal
              :visible="resetPasswordDialog"
              v-model="resetPasswordDialog"
              :closable="false"
            >
              <Button
                class="p-button-text"
                @click="closeResetPassword"
                label="Fermer"
                icon="pi pi-times"
              />
            </Dialog>
          </div>
        </div>
        <Button @click="onSubmit" type="submit" label="Submit" class="mt-2" />

        <Divider />
        <div v-if="error">
          <Message severity="error" :closable="false">{{ error }}</Message>
        </div>
        <div v-if="success">
          <Message severity="success" :closable="false">{{ success }}</Message>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Card from "primevue/button";
import InputText from "primevue/inputtext";
import Password from "primevue/password";
import Button from "primevue/button";
import Divider from "primevue/divider";
import Message from "primevue/message";
import Dialog from "primevue/dialog";
export default {
  name: "LoginForm",
  components: {
    Card,
    InputText,
    Password,
    Button,
    Divider,
    Message,
    Dialog,
  },
  data() {
    return {
      signUpDialog: false,
      resetPasswordDialog: false,
      email: null,
      password: null,
      success: null,
      error: null,
    };
  },
  methods: {
    onSubmit(e) {
      e.preventDefault();
      const userData = {
        email: this.email,
        password: this.password,
      };
      axios({
        method: "post",
        url: `${process.env.VUE_APP_URL_BACKEND}/api/auth/login`,
        data: userData,
        headers: {
          "Content-Type": "application/json",
        },
      })
        .then((res) => {
          localStorage.setItem("userConnected", JSON.stringify(res));
          this.success = "Connexion réussie";
          //setIsLogged(true);
          this.$router.push(`/${res.data.userId}`);
        })
        .catch((error) => {
          this.error = error.response.data.error;
        });
    },
    toggleSignUp() {
      this.signUpDialog = true;
    },
    closeSignUp() {
      this.signUpDialog = false;
    },
    toggleResetPassword() {
      this.resetPasswordDialog = true;
    },
    closeResetPassword() {
      this.resetPasswordDialog = false;
    },
  },
};
</script>

<style>
a {
  cursor: pointer;
}
.login {
  width: 100%;
  margin: 1em auto;
}
</style>
