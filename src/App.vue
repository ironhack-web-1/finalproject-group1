<template>
  <div class="container" style="padding: 50px 0 100px 0">
    <nav v-if="isAuthenticated">
      <router-link to="/">Home</router-link> |
      <router-link to="/profile">Profile</router-link>
    </nav>
    <Alert />
    <!-- <Auth v-else /> -->
    <router-view />
  </div>
</template>

<script>
import { computed, onUpdated } from "vue";
import { useRouter, useRoute } from "vue-router";
import { supabase } from "./supabase";
import { useUserStore } from "./store/user";
import Auth from "./components/Auth.vue";
import Alert from "./components/Alert.vue";
import Profile from "./pages/Profile.vue";

export default {
  components: {
    Auth,
    Alert,
    Profile,
  },

  setup() {
    const router = useRouter();
    const store = useUserStore();

    store.user = supabase.auth.user();
    supabase.auth.onAuthStateChange((_, session) => {
      store.user = session.user;
    });

    const isAuthenticated = computed(() => {
      return store.user !== null;
    });

    if (isAuthenticated.value) {
      router.push("/dashboard");
    } else {
      router.push("/auth");
    }

    onUpdated(() => {
      if (isAuthenticated.value) {
        router.push("/dashboard");
      } else {
        router.push("/auth");
      }
    });

    return {
      isAuthenticated,
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
