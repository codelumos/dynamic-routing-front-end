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
        特权密码
      </p>
    </v-row>

    <!-- 配置卡片 -->
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

    <!-- 统一密码选项 -->
    <p style="white-space: pre-line" v-show="!pwd_uf_en"></p>
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
      <v-col sm="3">
        <v-btn
            elevation="4"
            color="primary"
            @click="enable"
        >
          进入特权模式
        </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  name: "Privilege",
  data() {
    return {
      // 消息条
      show_snackbar: false,
      icon: 'mdi-minus-circle',
      snackbar_text: '网络连接失败',
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
      show_s2: false
    }
  },
  methods: {
    // 弹出消息条
    tip() {
      this.show_snackbar = true
      setTimeout(function () {
        // 重置消息条信息
        this.snackbar_text = "网络连接失败"
        this.icon = 'mdi-minus-circle'
        this.color = 'warning'
      }, 1000)
    },
    // 进入特权模式
    enable() {
      // 检查特权密码是否为空
      if ((this.pwd_uf_en && this.pwd_uf === '') || (!this.pwd_uf_en && (this.pwd_r0 === '' || this.pwd_r1 === '' || this.pwd_r2 === '' || this.pwd_s2 === ''))) {
        this.snackbar_text = '特权密码不能为空！'
        this.icon = 'mdi-alert-circle'
        this.color = 'warning'
        this.tip()
        return
      }
      const url = 'http://127.0.0.1:5000/enable'
      let data = {
        pwd_r0: this.pwd_uf ? this.pwd_uf : this.pwd_r0,
        pwd_r1: this.pwd_uf ? this.pwd_uf : this.pwd_r1,
        pwd_r2: this.pwd_uf ? this.pwd_uf : this.pwd_r2
      }
      axios({
        method: 'post',
        url: url,
        data: data
      }).then(res => {
        console.log(res)
        if (res.data.state) {
          this.snackbar_text = res.data.msg
          this.icon = 'mdi-checkbox-marked-circle'
          this.color = 'success'
        } else {
          this.snackbar_text = res.data.msg
          this.icon = 'mdi-cancel'
          this.color = 'error'
        }
        this.tip()
      }).catch(err => {
        console.log(err)
        this.tip()
      })
    }
  }
}
</script>
