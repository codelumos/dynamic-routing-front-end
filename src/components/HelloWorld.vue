<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
        <v-img
            :src="require('../assets/Topography.png')"
            class="my-3"
            contain
            height="400"
        />
      </v-col>

      <v-col class="mb-4">
        <h1 class="display-2 font-weight-bold mb-3">
          Welcome to Vuetify
        </h1>

        <p class="subheading font-weight-regular">
          For help and collaboration with other Vuetify developers
        </p>
      </v-col>

      <v-col class="mb-5" cols="12">
        <v-container>
          <v-row>
            <v-col cols="12" md="4">
              <v-text-field
                  v-model="ip_r0"
                  label="IP地址"
                  required
              ></v-text-field>
            </v-col>

            <v-col cols="12" md="4">
              <v-text-field
                  v-model="pwd_telnet_r0"
                  label="Telnet密码"
                  required
              ></v-text-field>
            </v-col>

            <v-col cols="12" md="4">
              <v-text-field
                  v-model="pwd_enable_r0"
                  label="特权密码"
                  required
              ></v-text-field>
            </v-col>
          </v-row>
          <v-btn color="primary" v-on:click="test">TEST</v-btn>
        </v-container>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios'

export default {
  name: 'HelloWorld',

  data() {
    return {
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
