<template>
  <form class="row flex-center flex" @submit.prevent="handleLogin">
    <div class="col-6 form-widget">
      <h1 class="header">Supabase + Vue 3</h1>
      <p class="description">Sign in via magic link with your email below</p>
      <div>
        <input
          class="inputField"
          type="email"
          placeholder="Your email"
          v-model="email"
        />
      </div>
      <div>
        <input
          type="submit"
          class="button block"
          :value="loading ? 'Loading' : 'Send magic link'"
          :disabled="loading"
        />
      </div>
    </div>
  </form>
</template>

<script>
import { ref, onMounted } from "vue";
import { supabase } from "../supabase";

export default {
  setup() {
    const loading = ref(false);
    const email = ref("");
    const password = ref("");

    const handleLogin = async () => {
      try {
        loading.value = true;
        const { user, session, error } = await supabase.auth.signIn({
          email: email.value,
        });
        console.log(user, session);
        if (error) throw error;
        alert("Check your email for the login link!");
      } catch (error) {
        alert(error.error_description || error.message);
      } finally {
        loading.value = false;
      }
    };

    return {
      loading,
      email,
      handleLogin,
      password,
      storeTask,
    };
  },
};
</script>
