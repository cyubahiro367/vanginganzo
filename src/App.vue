<template>
  <div>
    <router-view/>
    <vue-progress-bar></vue-progress-bar>
  </div>
</template>

<script>
import axios from "axios";
export default {
  mounted () {
    this.$Progress.finish()
  },
  created() {
    
    this.$store.commit('initializeStore')

    const token = this.$store.state.token

    if (token) {
      axios.defaults.headers.common['Authorization'] = "Bearer " + token
    } else {
      axios.defaults.headers.common['Authorization'] = ""
    }
  },
};
</script>
