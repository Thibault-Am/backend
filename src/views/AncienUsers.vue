<template>
  <div class="Users">
    <b-navbar class="is-light">
      <template #brand>
        <b-navbar-item tag="router-link" :to="{ path: '/home' }">
          <img
            src="https://etapes.com/app/uploads/2016/05/1464094938.png"
            alt="Atelier2"
          />
        </b-navbar-item>
        <b-navbar-item tag="router-link" :to="{ path: '/ancienusers' }">
          AncienUtilisateur
        </b-navbar-item>
        <b-navbar-item tag="router-link" :to="{ path: '/rdvinactif' }">
          RDVinactif
        </b-navbar-item>
        <b-navbar-item tag="router-link" :to="{ path: '/rdv' }">
          RDV
        </b-navbar-item>
      </template>
      <template #end>
        <b-navbar-item tag="div">
          <div>
            <b-button
              type="is-danger is-light"
              @click="deconnexion"
              class="button is-primary is-black"
              id="ButtonDeconnexion"
              >Deconnexion</b-button
            >
          </div>
        </b-navbar-item>
      </template>
    </b-navbar>
    <div v-for="user in users" :key="user.id">
      <p>
        {{ user.prenom }}
        {{ user.nom }}
      </p>
      {{ user.mail }}
      <button
        @click="deleteUserAbsent(user.id, user.mail)"
        class="delete is-medium is-vcentered suppButton"
        id="DeleteUser"
      ></button>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "Users",
  components: {},
  data() {
    return {
      users: null,
      error: null,
    };
  },
  methods: {
    deconnexion() {
      this.$store.state.token = null;
      this.$router.push("/");
    },

    chargementAncienUsers() {
      //console.log("je usis la ");
      axios
        .get(`http://149.91.80.75:19380/userAbsent`)
        .then((response) => (this.users = response.data));
    },
    deleteUserAbsent(id, label) {
      console.log("je suis la ");
      this.$buefy.dialog.confirm({
        type: "is-danger",
        cancelText: "Annuler",
        confirmText: "Accepter",
        message: `Supprimer l'utilisateur avec le mail <strong>${label}</strong> ?`,
        onConfirm: () => {
          axios
            .delete("http://149.91.80.75:19380/userSupp/" + id)
            .then((response) => {
              this.$buefy.toast.open("Utilisateur supprimée");
              axios
                .get("http://149.91.80.75:19380/userAbsent")
                .then((response) => {
                  this.users = response.data;
                });
            });
        },
      });
    },
  },
  created() {
    this.chargementAncienUsers();
  },
};
</script>

<style lang="scss">
#AncienUsers {
  margin-top: 7px;
}
</style>