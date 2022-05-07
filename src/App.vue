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
            width="170"
        />
      </div>
      <v-app-bar-title class="d-flex align-center">
        <h2>动态路由</h2>
      </v-app-bar-title>
    </v-app-bar>

    <!-- 主页面 -->
    <v-main>
      <div id="app">
        <!-- 全局Snackbar队列 -->
        <!-- 根据当前Snackbar的index动态计算Y方向的位移，达到依次排列的目的 -->
        <snackbar
            v-model="item.show"
            :style="{transform: 'translateY(' + 60 * index + 'px)'}"
            :message="item.content"
            v-for="(item, index) in messages"
            :key="item.id"
            @input="onSnackbarClose($event, index)">
        </snackbar>
      </div>

      <p style="white-space: pre-line"></p>
      <Topology/>
      <p style="white-space: pre-line"></p>

      <v-tabs centered>
        <v-tab>
          <v-icon left>
            mdi-account
          </v-icon>
          <h3>设备登录</h3>
        </v-tab>
        <v-tab>
          <v-icon left>
            mdi-account-cog
          </v-icon>
          <h3>特权模式</h3>
        </v-tab>
        <v-tab>
          <v-icon left>
            mdi-link-variant
          </v-icon>
          <h3>串行接口配置</h3>
        </v-tab>
        <v-tab>
          <v-icon left>
            mdi-source-branch-check
          </v-icon>
          <h3>连通性检测</h3>
        </v-tab>
        <v-tab>
          <v-icon left>
            mdi-sitemap
          </v-icon>
          <h3>路由配置</h3>
        </v-tab>

        <v-tab-item>
          <v-card flat>
            <Login/>
          </v-card>
        </v-tab-item>
        <v-tab-item>
          <v-card flat>
            <Privilege/>
          </v-card>
        </v-tab-item>
        <v-tab-item>
          <v-card flat>
            <Serial/>
          </v-card>
        </v-tab-item>
        <v-tab-item>
          <v-card flat>
            <Connectivity/>
          </v-card>
        </v-tab-item>
        <v-tab-item>
          <v-card flat>
            <Protocol/>
          </v-card>
        </v-tab-item>
      </v-tabs>

      <p style="white-space: pre-line"></p>

    </v-main>

    <!-- 底栏 -->
    <v-footer
        color="primary"
        dark
    >
      <v-card
          flat
          width="100%"
          class="primary text-center"
      >
        <span>
        Copyright &copy; <a href="mailto:haonshi@qq.com" style="color: white">Haonan Shi</a>,
        <a href="https://www.nju.edu.cn/" style="color: white">Nanjing University</a>
        </span>
      </v-card>
    </v-footer>
  </v-app>
</template>

<script>
import Snackbar from "./components/Snackbar.vue";
import Topology from "./components/Topology.vue";
import Login from "./components/Login.vue";
import Privilege from "./components/Privilege.vue";
import Serial from "./components/Serial.vue";
import Connectivity from "./components/Connectivity.vue";
import Protocol from "./components/Protocol.vue";

export default {
  name: "App",

  components: {
    Connectivity,
    Snackbar,
    Login,
    Privilege,
    Serial,
    Topology,
    Protocol
  },
  data: function () {
    return {
      messages: [] // 消息队列
    }
  },
  mounted() {
    // 全局Snackbar控制器事件监听
    this.snackbarController()
  },
  methods: {
    snackbarController() {
      // 监听showSnackbar事件
      this.$eventBus.$on('showSnackbar', data => {
        // 将收到的message推入messages数组中
        this.messages.push({
          ...data,
          show: true
        })
      })
    },
    onSnackbarClose(value, index) {
      // value为Snackbar组件内部传递出来的
      // index为当前关闭Snackbar的索引
      // 删除已关闭的Snackbar对应的消息数据
      this.messages.splice(index, 1)
    }
  }
};
</script>
