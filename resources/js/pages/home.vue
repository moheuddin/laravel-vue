<template>

    <div class="container">
      <Usernav/>

      <router-view />
    </div>

</template>

<script>
import { mapGetters } from 'vuex'
import Usernav from '../components/Usernav'

export default {
  components: {
    Usernav: Usernav
  },

  data: () => ({
    appName: window.config.appName
  }),

  computed: mapGetters({
    user: 'auth/user'
  }),

  methods: {
    async logout () {
      // Log out the user.
      await this.$store.dispatch('auth/logout')

      // Redirect to login.
      this.$router.push({ name: 'login' })
    }
  }
}
</script>

<style scoped>
.user-nav {
  list-style: none;
  padding:0;
  margin: 0;
  text-align: center;
  display: flex;
}
</style>
