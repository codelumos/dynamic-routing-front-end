<template>
  <v-app>
    <!-- 顶栏 -->
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
      <v-app-bar-title class="d-flex align-center">
        <h2>动态路由</h2>
      </v-app-bar-title>
    </v-app-bar>

    <!-- 主页面 -->
    <v-main>
      <p style="white-space: pre-line"></p>
      <Topology/>
      <Login/>
      <p style="white-space: pre-line"></p>
      <Config/>
      <p style="white-space: pre-line"></p>
    </v-main>

    <!-- 底栏 -->
    <v-footer
        color="primary"
        dark
    >
      <div class="align-center">
        DynamicRouter
      </div>
    </v-footer>
  </v-app>
</template>

<script>
import Login from './components/Login';
import Topology from './components/Topology';
import Config from "./components/Config";

export default {
  name: 'App',

  components: {
    Login,
    Topology,
    Config
  },
};
</script>
