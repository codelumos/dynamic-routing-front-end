<template>
  <v-container>
    <!-- 消息条 -->
    <v-snackbar
        v-model="show_snackbar"
        :timeout="timeout"
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
        路由配置
      </p>
    </v-row>

    <p style="white-space: pre-line"></p>

    <!-- 卡片 -->
    <v-row class="text-center" v-show="!pwd_uf_en">
      <v-col sm="3">
        <v-card elevation="4">
          <v-card-title>Switch2</v-card-title>
          <v-card-text>
            <v-text-field
                v-model="pwd_s2"
                label="特权密码"
                required
                outlined
                :append-icon="show_s2 ? 'mdi-eye' : 'mdi-eye-off'"
                :type="show_s2 ? 'text' : 'password'"
                @click:append="show_s2 = !show_s2"
            ></v-text-field>
          </v-card-text>
        </v-card>
      </v-col>

      <v-spacer></v-spacer>

      <v-col sm="3">
        <v-card elevation="4">
          <v-card-title>Router0</v-card-title>
          <v-card-text>
            <v-text-field
                v-model="pwd_r0"
                label="特权密码"
                required
                outlined
                :append-icon="show_r0 ? 'mdi-eye' : 'mdi-eye-off'"
                :type="show_r0 ? 'text' : 'password'"
                @click:append="show_r0 = !show_r0"
            ></v-text-field>
          </v-card-text>
        </v-card>
      </v-col>

      <v-spacer></v-spacer>

      <v-col sm="3">
        <v-card elevation="4">
          <v-card-title>Router1</v-card-title>
          <v-card-text>
            <v-text-field
                v-model="pwd_r1"
                label="特权密码"
                required
                outlined
                :append-icon="show_r1 ? 'mdi-eye' : 'mdi-eye-off'"
                :type="show_r1 ? 'text' : 'password'"
                @click:append="show_r1 = !show_r1"
            ></v-text-field>
          </v-card-text>
        </v-card>
      </v-col>

      <v-spacer></v-spacer>

      <v-col sm="3">
        <v-card elevation="4">
          <v-card-title>Router2</v-card-title>
          <v-card-text>
            <v-text-field
                v-model="pwd_r2"
                label="特权密码"
                required
                outlined
                :append-icon="show_r2 ? 'mdi-eye' : 'mdi-eye-off'"
                :type="show_r2 ? 'text' : 'password'"
                @click:append="show_r2 = !show_r2"
            ></v-text-field>
          </v-card-text>
        </v-card>
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
            label="统一特权密码"
            required
            outlined
            :append-icon="show_uf ? 'mdi-eye' : 'mdi-eye-off'"
            :type="show_uf ? 'text' : 'password'"
            @click:append="show_uf = !show_uf"
        ></v-text-field>
      </v-col>
    </v-row>

    <p style="white-space: pre-line"></p>

    <v-row>
      <v-col sm="10">
        <v-select
            v-model="selected_protocol"
            :items="protocol_table"
            label="路由协议"
            outlined
        ></v-select>
      </v-col>
      <v-col sm="2">
        <v-btn elevation="4" color="primary" v-on:click="config">
          配置路由
        </v-btn>
      </v-col>
    </v-row>

    <v-row>
      <v-col cols="12">
        <v-textarea
            color="teal"
            outlined
            readonly
            v-model="msg"
        >
          <template v-slot:label>
            <div>
              控制台
            </div>
          </template>
        </v-textarea>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios'

export default {
  name: "Config",
  data() {
    return {
      // 消息条
      show_snackbar: false,
      icon: 'mdi-minus-circle',
      snackbar_text: '网络连接失败',
      timeout: 2000,
      color: 'warning',
      // 使用统一特权密码
      pwd_uf_en: true,
      pwd_uf: '',
      // 设备独立特权密码
      pwd_r0: '',
      pwd_r1: '',
      pwd_r2: '',
      pwd_s2: '',
      // 密码可见性
      show_uf: false,
      show_r0: false,
      show_r1: false,
      show_r2: false,
      show_s2: false,
      // 路由协议表
      selected_protocol: 'RIP',
      protocol_table: ['RIP', 'OSPF', 'BGP'],
      // 返回信息
      msg: ''
    }
  },
  methods: {
    // 配置协议
    config() {
      //初始化信息条
      this.snackbar_text = '配置失败'
      this.icon = 'mdi-cancel'
      this.color = 'error'

      let data = {
        pwd_r0: this.pwd_uf ? this.pwd_uf : this.pwd_r0,
        pwd_r1: this.pwd_uf ? this.pwd_uf : this.pwd_r1,
        pwd_r2: this.pwd_uf ? this.pwd_uf : this.pwd_r2
      }
      let url = 'http://127.0.0.1:5000/config_' + this.selected_protocol.toLowerCase()
      // 调用接口
      axios({
        method: 'post',
        url: url,
        data: data
      }).then(res => {
        console.log(res)
        // 输出信息到控制台
        let result = res.data
        this.msg += result.msg + '\n'
        if (result.state) {
          this.snackbar_text = '配置完成'
          this.icon = 'mdi-checkbox-marked-circle'
          this.color = 'success'
        }
        // 弹出消息条
        this.show_snackbar = true
      }).catch(err => {
        console.log(err)
        // 弹出消息条
        this.show_snackbar = true
      })
    }
  }
}
</script>
