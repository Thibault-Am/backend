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

    <div v-for="r in rdv" :key="r.id">
      <div v-if="r.date < date">
        <p>
          {{ r.libelle_event }}<br />
          {{ r.libelle_lieu }}

          <button
            @click="deleteAncienRdv(r.id, r.libelle_event)"
            class="delete is-medium is-vcentered suppButton"
            id="DeleteRdv"
          ></button>
        </p>
      </div>
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
      date: null,
      rdv: null,
      error: null,
    };
  },
  methods: {
    deconnexion() {
      this.$store.state.token = null;
      this.$router.push("/");
    },

    chargementAncienRdv() {
      axios
        .get(`http://149.91.80.75:19380/rdv`)
        .then((response) => (this.rdv = response.data));
    },
    deleteAncienRdv(id, label) {
      console.log("je suis la ");
      this.$buefy.dialog.confirm({
        type: "is-danger",
        cancelText: "Annuler",
        confirmText: "Accepter",
        message: `Supprimer le rdv avec le libelle <strong>${label}</strong> ?`,
        onConfirm: () => {
          axios
            .delete("http://149.91.80.75:19380/rdvSupp/" + id)
            .then((response) => {
              this.$buefy.toast.open("RDV supprimée");
              axios.get("http://149.91.80.75:19380/rdv").then((response) => {
                this.rdv = response.data;
              });
            });
        },
      });
    },
  },
  created() {
    this.chargementAncienRdv();
    const today = new Date();
    if (today.getMonth() + 1 < 10) {
      this.date =
        today.getFullYear() +
        "-0" +
        (today.getMonth() + 1) +
        "-" +
        today.getDate();
    } else {
      this.date =
        today.getFullYear() +
        "-" +
        (today.getMonth() + 1) +
        "-" +
        today.getDate();
    }
  },
};
</script>

<style lang="scss">
#AncienUsers {
  margin-top: 7px;
}
</style>