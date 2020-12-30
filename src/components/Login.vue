<template>
  <v-container>
    <v-row>
      <p class="title font-weight-bold mb-3">
        设备登陆
      </p>
    </v-row>

    <p style="white-space: pre-line"></p>

    <v-row class="text-center">
      <v-col sm="3">
        <v-card elevation="4">
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
              :value="login_state_s2"
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
      </v-col>

      <v-spacer></v-spacer>

      <v-col sm="3">
        <v-card elevation="4">
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
              :value="login_state_r0"
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
      </v-col>

      <v-spacer></v-spacer>

      <v-col sm="3">
        <v-card elevation="4">
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
              :value="login_state_r1"
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
      </v-col>

      <v-spacer></v-spacer>

      <v-col sm="3">
        <v-card elevation="4">
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
              :value="login_state_r2"
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
      </v-col>
    </v-row>

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
            v-on:click="login_all"
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
      // IP地址
      ip_s2: '127.16.0.1',
      ip_r0: '127.16.0.2',
      ip_r1: '127.16.0.3',
      ip_r2: '127.16.0.4',
      // 使用统一密码
      pwd_uf_en: true,
      pwd_uf: '',
      // 设备独立密码
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
      // 登陆状态
      login_state_s2: true,
      login_state_r0: true,
      login_state_r1: true,
      login_state_r2: true
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
        console.log(res)
      }).catch(err => {
        console.log(err)
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
        console.log(res)
      }).catch(err => {
        console.log(err)
      })
    },
    login_all() {
      const url = 'http://127.0.0.1:5000/login'
      let params = {
        pwd: this.pwd_uf
      }
      axios({
        method: 'post',
        url: url,
        data: params
      }).then(res => {
        console.log(res)
      }).catch(err => {
        console.log(err)
      })
    }
  }
}
</script>
