<template>
  <v-container>
    <!-- 消息条 -->
    <v-snackbar
        v-model="show_snackbar"
        :top="true"
        :color="color"
    >
      <v-icon left>
        {{ icon }}
      </v-icon>
      {{ snackbar_text }}
      <template v-slot:action="{ attrs }">
        <v-btn
            icon
            v-bind="attrs"
            @click="show_snackbar = false"
        >
          <v-icon>
            mdi-window-close
          </v-icon>
        </v-btn>
      </template>
    </v-snackbar>

    <!-- 标题 -->
    <v-row>
      <p class="title font-weight-bold mb-3">
        设备登陆
      </p>
    </v-row>
    <p style="white-space: pre-line"></p>

    <!-- 卡片 -->
    <v-row class="text-center">
      <v-col sm="3">
        <v-hover v-slot="{ hover }">
          <v-card :elevation="hover ? 12 : 4">
            <v-card-title>Switch2</v-card-title>
            <v-card-text>
              <v-text-field
                  v-model="ip_s2"
                  label="IP地址"
                  required
                  outlined
                  disabled
              ></v-text-field>
              <v-text-field
                  v-show="!pwd_uf_en"
                  v-model="pwd_s2"
                  label="Telnet密码"
                  required
                  outlined
                  :append-icon="show_s2 ? 'mdi-eye' : 'mdi-eye-off'"
                  :type="show_s2 ? 'text' : 'password'"
                  @click:append="show_s2 = !show_s2"
              ></v-text-field>
              <v-btn
                  elevation="4"
                  color="primary"
                  @click="login('s2', ip_s2, pwd_uf_en ? pwd_uf : pwd_s2)"
              >
                登陆
              </v-btn>
            </v-card-text>
            <v-overlay
                :absolute=true
                :z-index=0
                :value="state_s2"
            >
              <v-btn
                  elevation="4"
                  color="red lighten"
                  @click="logout('s2')"
              >
                退出登陆
              </v-btn>
            </v-overlay>
          </v-card>
        </v-hover>
      </v-col>

      <v-spacer></v-spacer>

      <v-col sm="3">
        <v-hover v-slot="{ hover }">
          <v-card :elevation="hover ? 12 : 4">
            <v-card-title>Router0</v-card-title>
            <v-card-text>
              <v-text-field
                  v-model="ip_r0"
                  label="IP地址"
                  required
                  outlined
                  disabled
              ></v-text-field>
              <v-text-field
                  v-show="!pwd_uf_en"
                  v-model="pwd_r0"
                  label="Telnet密码"
                  required
                  outlined
                  :append-icon="show_r0 ? 'mdi-eye' : 'mdi-eye-off'"
                  :type="show_r0 ? 'text' : 'password'"
                  @click:append="show_r0 = !show_r0"
              ></v-text-field>
              <v-btn
                  elevation="4"
                  color="primary"
                  @click="login('r0', ip_r0, pwd_uf_en ? pwd_uf : pwd_r0)"
              >
                登陆
              </v-btn>
            </v-card-text>
            <v-overlay
                :absolute=true
                :z-index=0
                :value="state_r0"
            >
              <v-btn
                  elevation="4"
                  color="red lighten"
                  @click="logout('r0')"
              >
                退出登陆
              </v-btn>
            </v-overlay>
          </v-card>
        </v-hover>
      </v-col>

      <v-spacer></v-spacer>

      <v-col sm="3">
        <v-hover v-slot="{ hover }">
          <v-card :elevation="hover ? 12 : 4">
            <v-card-title>Router1</v-card-title>
            <v-card-text>
              <v-text-field
                  v-model="ip_r1"
                  label="IP地址"
                  required
                  outlined
                  disabled
              ></v-text-field>
              <v-text-field
                  v-show="!pwd_uf_en"
                  v-model="pwd_r1"
                  label="Telnet密码"
                  required
                  outlined
                  :append-icon="show_r1 ? 'mdi-eye' : 'mdi-eye-off'"
                  :type="show_r1 ? 'text' : 'password'"
                  @click:append="show_r1 = !show_r1"
              ></v-text-field>
              <v-btn
                  elevation="4"
                  color="primary"
                  @click="login('r1', ip_r1, pwd_uf_en ? pwd_uf : pwd_r1)"
              >
                登陆
              </v-btn>
            </v-card-text>
            <v-overlay
                :absolute=true
                :z-index=0
                :value="state_r1"
            >
              <v-btn
                  elevation="4"
                  color="red lighten"
                  @click="logout('r1')"
              >
                退出登陆
              </v-btn>
            </v-overlay>
          </v-card>
        </v-hover>
      </v-col>

      <v-spacer></v-spacer>

      <v-col sm="3">
        <v-hover v-slot="{ hover }">
          <v-card :elevation="hover ? 12 : 4">
            <v-card-title>Router2</v-card-title>
            <v-card-text>
              <v-text-field
                  v-model="ip_r2"
                  label="IP地址"
                  required
                  outlined
                  disabled
              ></v-text-field>
              <v-text-field
                  v-show="!pwd_uf_en"
                  v-model="pwd_r2"
                  label="Telnet密码"
                  required
                  outlined
                  :append-icon="show_r2 ? 'mdi-eye' : 'mdi-eye-off'"
                  :type="show_r2 ? 'text' : 'password'"
                  @click:append="show_r2 = !show_r2"
              ></v-text-field>
              <v-btn
                  elevation="4"
                  color="primary"
                  @click="login('r2', ip_r2, pwd_uf_en ? pwd_uf : pwd_r2)"
              >
                登陆
              </v-btn>
            </v-card-text>
            <v-overlay
                :absolute=true
                :z-index=0
                :value="state_r2"
            >
              <v-btn
                  elevation="4"
                  color="red lighten"
                  @click="logout('r2')"
              >
                退出登陆
              </v-btn>
            </v-overlay>
          </v-card>
        </v-hover>
      </v-col>
    </v-row>

    <!-- 配置统一密码 -->
    <v-row>
      <v-col sm="3">
        <v-switch
            v-model="pwd_uf_en"
            :label="`使用统一密码`"
        ></v-switch>
      </v-col>
      <v-col sm="6">
        <v-text-field
            v-show="pwd_uf_en"
            v-model="pwd_uf"
            label="统一Telnet密码"
            required
            outlined
            :append-icon="show_uf ? 'mdi-eye' : 'mdi-eye-off'"
            :type="show_uf ? 'text' : 'password'"
            @click:append="show_uf = !show_uf"
        ></v-text-field>
      </v-col>
      <v-col sm="3">
        <v-btn
            v-show="pwd_uf_en"
            elevation="4"
            color="primary"
            @click="login_all"
        >
          一键登录
        </v-btn>
      </v-col>
    </v-row>

  </v-container>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Login',

  data() {
    return {
      // 消息条
      show_snackbar: false,
      icon: 'mdi-minus-circle',
      snackbar_text: '网络连接失败',
      timeout: 2000,
      color: 'warning',
      // 设备IP地址
      ip_s2: '127.16.0.1',
      ip_r0: '127.16.0.2',
      ip_r1: '127.16.0.3',
      ip_r2: '127.16.0.4',
      // 使用统一telnet密码
      pwd_uf_en: true,
      pwd_uf: '',
      // 设备独立telnet密码
      pwd_s2: '',
      pwd_r0: '',
      pwd_r1: '',
      pwd_r2: '',
      // 密码可见性
      show_uf: false,
      show_s2: false,
      show_r0: false,
      show_r1: false,
      show_r2: false,
      // 设备登陆状态(true:已登陆, false:未登陆)
      state_s2: false,
      state_r0: false,
      state_r1: false,
      state_r2: false
    }
  },
  methods: {
    login(dev_no, ip, pwd) {
      const url = 'http://127.0.0.1:5000/login'
      let params = {
        dev_no: dev_no,
        ip: ip,
        pwd: pwd
      }
      axios({
        method: 'post',
        url: url,
        data: params
      }).then(res => {
        console.log(res);
        if (res.data.state) {
          // 将设备状态设为登陆状态
          this.set_dev_state(dev_no, true)
          // 设置消息条
          this.snackbar_text = "登陆成功"
          this.icon = 'mdi-checkbox-marked-circle'
          this.color = 'success'
        } else {
          // 将设备状态设为未登陆状态
          this.set_dev_state(dev_no, false)
          // 设置消息条
          this.snackbar_text = "登陆失败"
          this.icon = 'mdi-cancel'
          this.color = 'error'
        }
        // 弹出消息条
        this.show_snackbar = true
      }).catch(err => {
        console.log(err)
        // 弹出消息条
        this.show_snackbar = true
      })
    },
    logout(dev_no) {
      const url = 'http://127.0.0.1:5000/logout'
      let params = {
        dev_no: dev_no
      }
      axios({
        method: 'post',
        url: url,
        data: params
      }).then(res => {
        console.log(res);
        if (res.data.state) {
          // 将设备状态设为未登陆状态
          this.set_dev_state(dev_no, false)
          this.snackbar_text = "登出成功"
          this.icon = 'mdi-checkbox-marked-circle'
          this.color = 'success'
        } else {
          // 设备状态保持登陆状态
          this.set_dev_state(dev_no, true)
          this.snackbar_text = "登出失败"
          this.icon = 'mdi-cancel'
          this.color = 'error'
        }
        this.show_snackbar = true
      }).catch(err => {
        console.log(err)
        this.show_snackbar = true
      })
    },
    login_all() {
      axios.all([this.login("s2", this.ip_s2, this.pwd_uf),
        this.login("r0", this.ip_r0, this.pwd_uf),
        this.login("r1", this.ip_r1, this.pwd_uf),
        this.login("r2", this.ip_r2, this.pwd_uf),
      ]).then(axios.spread(function (res) {
        console.log(res);
      })).catch(err => {
        console.log(err)
      })
    },
    set_dev_state(dev_no, state) {
      switch (dev_no) {
        case "s2":
          this.state_s2 = state;
          break;
        case "r0":
          this.state_r0 = state;
          break;
        case "r1":
          this.state_r1 = state;
          break;
        case "r2":
          this.state_r2 = state;
          break;
      }
    }
  }
}
</script>
