<template>
  <v-app>
    <v-app-bar
        app
        color="primary"
        dark
    >
      <div class="d-flex align-center">
        <v-img
            alt="Vuetify Logo"
            class="shrink mr-2"
            contain
            src="./assets/logo.png"
            transition="scale-transition"
            height="50"
        />
      </div>
      <div class="d-flex align-center">
        <h1>动态路由</h1>
      </div>
    </v-app-bar>

    <v-main>
      <Topology/>
      <Login/>
    </v-main>
  </v-app>
</template>

<script>
import Login from './components/Login';
import Topology from './components/Topology';

export default {
  name: 'App',

  components: {
    Login,
    Topology,
  },

  data: () => ({
    //
  }),
};
</script>
