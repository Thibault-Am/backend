<template>
  <div class="Users">
    <b-navbar class="is-light">
      <template #brand > 
        <b-navbar-item tag="router-link" :to="{ path: '/home' }">
            <img
                src="https://etapes.com/app/uploads/2016/05/1464094938.png"
                alt="Atelier2"
            >
        </b-navbar-item>
        <b-navbar-item tag="router-link" :to="{ path: '/ancienusers' }">
          AncienUtilisateur
        </b-navbar-item>

      </template>
      <template #end >
        <b-navbar-item tag="div">
          <div>
            <b-button type="is-danger is-light" @click="deconnexion" class="button is-primary is-black" id="ButtonDeconnexion">Deconnexion</b-button>
          </div>
        </b-navbar-item>
      </template>
    </b-navbar>

  </div>
  
</template>
<script>

import axios from 'axios';
export default {
  name: "Users",
  components: {},
  data() {
    return {
      users:null,
      error: null,
    };
  },
  methods:{
    deconnexion(){
      this.$store.state.token = null;
      this.$router.push("/");
    },
  },

}

</script>

<style lang="scss">

#AncienUsers{
  margin-top: 7px;
}

</style>