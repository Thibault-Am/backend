<template>
<div id="app">
<router-view/>
</div>
</template>

<script>
import { defineComponent } from '@vue/composition-api'

export default defineComponent({
  
  methods:{
    deconnexion(){
      this.$store.state.token = null;
      this.$router.push("/");
    },
  },
})
</script>

<style lang="scss">

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}


#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
