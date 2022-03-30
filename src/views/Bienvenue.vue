<template>
  <div class="Users">
    <b-navbar class="is-light">
      <template #brand > 
        <b-navbar-item >
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
    <div v-for="user in users" :key="user.id" >
        <p>
        {{user.prenom}}
       {{user.nom}}
      </p>
      {{user.mail}}
         <button @click="deleteUser(conv.id, conv.label)" class="delete is-medium is-vcentered suppButton" id="DeleteUser"></button>
    </div>
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
   
    chargementusers(){
       axios.get(`http://149.91.80.75:19380/users`)
     .then((response) => this.users= response.data)},

     AncienUsers(){
      this.$store.state.token = null;
      this.$router.push("/ancienusers");

    },
    deleteUser(){
      axios.get(`http://149.91.80.75:19380/users`)
      this.users.dialog.confirm({
  
        type: 'is-danger',
        cancelText: 'Annuler',
        confirmText: 'Accepter',
        message: `Supprimer l'utilisateur <strong>${label}</strong> ?`,
        onConfirm: () => {
          this.users.delete().then(response => {
            
            this.$buefy.toast.open('Conversation supprimée')
            this.users.get().then(response => {
              this.users = response.data 
            })
          })
        }
      })
    }
     
  },

  
  
  created(){
    this.chargementusers();
  },

}

</script>

<style lang="scss">

.title{
  color:#48C78E;
}

#AncienUsers{
  margin-top: 7px;
}

#DeleteUser{
  margin-left: 70px;
 margin-bottom: 20px
}

</style>