<template>
  <v-container>
    <v-row class="text-center">
      <v-card elevation="4">
        <v-card-title>Router0</v-card-title>
        <v-card-text>
          <v-text-field
              v-model="ip_r0"
              label="IP地址"
              required
              outlined
          ></v-text-field>
          <v-text-field
              v-model="pwd_telnet_r0"
              label="Telnet密码"
              required
              outlined
              :append-icon="show_r0_1 ? 'mdi-eye' : 'mdi-eye-off'"
              :type="show_r0_1 ? 'text' : 'password'"
              @click:append="show_r0_1 = !show_r0_1"
          ></v-text-field>
          <v-text-field
              v-model="pwd_enable_r0"
              label="特权密码"
              required
              outlined
              :append-icon="show_r0_2 ? 'mdi-eye' : 'mdi-eye-off'"
              :type="show_r0_2 ? 'text' : 'password'"
              @click:append="show_r0_2 = !show_r0_2"
          ></v-text-field>
          <v-btn elevation="2" color="primary" v-on:click="test">
            测试登陆
          </v-btn>
        </v-card-text>
      </v-card>

      <v-spacer></v-spacer>

      <v-card elevation="4">
        <v-card-title>Router1</v-card-title>
        <v-card-text>
          <v-text-field
              v-model="ip_r0"
              label="IP地址"
              required
              outlined
          ></v-text-field>
          <v-text-field
              v-model="pwd_telnet_r0"
              label="Telnet密码"
              required
              outlined
              :append-icon="show_r1_1 ? 'mdi-eye' : 'mdi-eye-off'"
              :type="show_r1_1 ? 'text' : 'password'"
              @click:append="show_r1_1 = !show_r1_1"
          ></v-text-field>
          <v-text-field
              v-model="pwd_enable_r0"
              label="特权密码"
              required
              outlined
              :append-icon="show_r1_2 ? 'mdi-eye' : 'mdi-eye-off'"
              :type="show_r1_2 ? 'text' : 'password'"
              @click:append="show_r1_2 = !show_r1_2"
          ></v-text-field>
          <v-btn elevation="2" color="primary" v-on:click="test">
            测试登陆
          </v-btn>
        </v-card-text>
      </v-card>

      <v-spacer></v-spacer>

      <v-card elevation="4">
        <v-card-title>Router2</v-card-title>
        <v-card-text>
          <v-text-field
              v-model="ip_r0"
              label="IP地址"
              required
              outlined
          ></v-text-field>
          <v-text-field
              v-model="pwd_telnet_r0"
              label="Telnet密码"
              required
              outlined
              :append-icon="show_r2_1 ? 'mdi-eye' : 'mdi-eye-off'"
              :type="show_r2_1 ? 'text' : 'password'"
              @click:append="show_r2_1 = !show_r2_1"
          ></v-text-field>
          <v-text-field
              v-model="pwd_enable_r0"
              label="特权密码"
              required
              outlined
              :append-icon="show_r2_2 ? 'mdi-eye' : 'mdi-eye-off'"
              :type="show_r2_2 ? 'text' : 'password'"
              @click:append="show_r2_2 = !show_r2_2"
          ></v-text-field>
          <v-btn elevation="2" color="primary" v-on:click="test">
            测试登陆
          </v-btn>
        </v-card-text>
      </v-card>

      <v-spacer></v-spacer>

      <v-card elevation="4">
        <v-card-title>Switch2</v-card-title>
        <v-card-text>
          <v-text-field
              v-model="ip_r0"
              label="IP地址"
              required
              outlined
          ></v-text-field>
          <v-text-field
              v-model="pwd_telnet_r0"
              label="Telnet密码"
              required
              outlined
              :append-icon="show_s2_1 ? 'mdi-eye' : 'mdi-eye-off'"
              :type="show_s2_1 ? 'text' : 'password'"
              @click:append="show_s2_1 = !show_s2_1"
          ></v-text-field>
          <v-text-field
              v-model="pwd_enable_r0"
              label="特权密码"
              required
              outlined
              :append-icon="show_s2_2 ? 'mdi-eye' : 'mdi-eye-off'"
              :type="show_s2_2 ? 'text' : 'password'"
              @click:append="show_s2_2 = !show_s2_2"
          ></v-text-field>
          <v-btn elevation="2" color="primary" v-on:click="test">
            测试登陆
          </v-btn>
        </v-card-text>
      </v-card>
    </v-row>
  </v-container>

</template>

<script>
import axios from 'axios'

export default {
  name: 'Login',

  data() {
    return {
      show_r0_1: false,
      show_r0_2: false,
      show_r1_1: false,
      show_r1_2: false,
      show_r2_1: false,
      show_r2_2: false,
      show_s2_1: false,
      show_s2_2: false,
      ip_r0: '',
      pwd_telnet_r0: '',
      pwd_enable_r0: ''
    }
  },
  methods: {
    //前端调用测试
    test() {
      const url = 'http://127.0.0.1:5000/test'
      let params = {
        ip: '1.1.1.1',
        pwd: '12345'
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
