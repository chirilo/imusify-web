<template>
  <nav class="navbar">
    <div>
      <a href="#" class="hamburger" @click.prevent="toggleSideBar()">
        <icon name="hamburger" />
      </a>
      <router-link to="/" class="logo">
        <icon name="logo" />
      </router-link>
    </div>
    <div class="subnav">
      <a href="#"
         @click.prevent="toggleLogin(true)"
         class="button"
         v-if="!isLoggedIn">{{$t('login')}}</a>
      <a href="#"
         @click.prevent="toggleSignup(true)"
         class="button"
         v-if="!isLoggedIn">{{$t('signup')}}</a>
      <a href="#" @click.prevent="logout()" class="button"  v-if="isLoggedIn">{{$t('logout')}}</a>
    </div>
  </nav>
</template>
<script>
import { mapActions, mapGetters, mapMutations } from 'vuex';
import Icon from '@/components/Icon.vue';
import * as types from '../store/types';

export default {
  name: 'NavBar',
  components: {
    Icon,
  },
  props: ['isSideBarOpen'],
  computed: {
    ...mapGetters({
      user: types.ACCOUNTS_USER,
      token: types.ACCOUNTS_TOKEN,
      isSignupOpen: types.TOGGLER_SIGNUP,
      isLoginOpen: types.TOGGLER_LOGIN,
    }),

    isLoggedIn() {
      return Boolean(this.token);
    },
  },

  methods: {
    ...mapActions({
      logout: types.ACCOUNTS_LOGOUT,
    }),

    ...mapMutations({
      toggleSignup: types.TOGGLER_SIGNUP,
      toggleLogin: types.TOGGLER_LOGIN,
    }),

    toggleSideBar() {
      this.isSideBarOpen = !this.isSideBarOpen;
      this.$emit('toggleSideBar', this.isSideBarOpen);
    },
  },
};
</script>
<style lang="scss" scoped>
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 0;

  .button {
    text-align: center;
  }

  .hamburger, .logo {
    margin-right: 1rem;
  }

  > div {
    display: flex;
    align-items: center;
  }
}

@media (min-width: 480px) {
  .navbar {
    padding: 1rem;
    flex-direction: column;
    width: 19rem;
    justify-content: flex-start;
    background: #22201e url("/sidebar-bg.png") no-repeat center center fixed;
    background-size: cover;

    .subnav {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;

      .button {
        margin: 0 0 2.1rem 0;
        width: 15rem;
      }
    }
  }

  .hamburger {
    display: none;
  }
}
</style>
