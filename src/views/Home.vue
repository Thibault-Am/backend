<template>
  <div id="bc" class="login box column is-half is-offset-one-quarter mt-6">
    <form @submit.prevent="login">
      <h1 class="title is-4 has-text-centered">Login</h1>

      <b-field label="Email :">
        <b-input
          type="email"
          maxlength="30"
          v-model="email"
          required
          placeholder="Ex : theo@mail.fr"
        >
        </b-input>
      </b-field>

      <b-field label="Password :">
        <b-input
          type="password"
          v-model="password"
          required
          placeholder="Ex : fhq7h"
        >
        </b-input>
      </b-field>

      <div>
        <div class="has-text-centered mt-5 mb-3">
          <button class="button" type="is-success" id="Valider">valid</button>
        </div>
      </div>
      <p>{{ error }}</p>
    </form>
  </div>
</template>
<script>
const axios = require("axios");

export default {
  name: "Login",
  components: {},
  data() {
    return {
      email: null,
      password: null,
      error: null,
    };
  },
  methods: {
    switchToLogin: function () {
      this.mode = "login";
    },
    login() {
      axios
        .get(`http://149.91.80.75:19380/auth`, {
          auth: {
            username: this.email,
            password: this.password,
          },
        })
        .then((response) => {
          this.$store.commit("setToken", response.data.token);
          this.$store.commit("setAccess_token", response.data.access_token);
          this.$router.push("/home");
          // this.error = "success to connect";
        })
        .then((response) => console.log(response))
        .catch((error) => {
          this.$store.commit("setToken", null),
            (this.error = error.response.data.message);
        });
    },
    deconnexion() {
      this.$store.state.token = null;
      this.$router.push("/");
    },
  },

  created() {},
};
</script>

<style scoped>
.title {
  color: #48c78e;
}
#Valider {
  border: 1px solid #48c78e;
  background-color: #48c78e;
  color: white;
}
#Valider:hover {
  background-color: white;
  color: black;
}
</style>