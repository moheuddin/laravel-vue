<template>
  <div>
          <!-- Authenticated -->
          <ul class="user-nav" >

          <li>
              <router-link :to="{ name: 'team.index' }" class="dropdown-item pl-3">
               <i class="fa fa-plus-circle"></i>
              My Team
              </router-link>
          </li>
          <li>
              <router-link :to="{ name: 'team.create' }" class="dropdown-item pl-3">
               <i class="fa fa-plus-circle"></i>
               Create Team
              </router-link>
          </li>
          <li>
              <router-link :to="{ name: 'team.update' }" class="dropdown-item pl-3">
               <i class="fa fa-plus-circle"></i>
               Update Team
              </router-link>
          </li>

            </ul>

  </div>
</template>

<script>
import { mapGetters } from 'vuex'

export default {

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
<style>
ul.user-nav {
    /* text-align: right; */
    list-style: none;
    padding: 0;
    margin: 0;
    display: flex;
    justify-content: flex-end;
    background: #ddd;
    margin-bottom: 10px;
}
</style>
