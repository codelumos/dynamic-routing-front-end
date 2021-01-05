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
        串行接口配置
      </p>
    </v-row>

    <!-- 配置卡片 -->
    <v-row class="text-center">
      <v-col sm="4">
        <v-hover v-slot="{ hover }">
          <v-card :elevation="hover ? 12 : 4">
            <v-card-title>Router0</v-card-title>
            <v-card-text>
              <v-text-field
                  v-model="serial0_r0"
                  label="Serial0/0/0"
                  required
                  outlined
                  disabled
              ></v-text-field>
              <v-text-field
                  v-model="serial1_r0"
                  label="Serial0/0/1"
                  required
                  outlined
                  disabled
              ></v-text-field>
              <v-text-field
                  v-model="mask_r0"
                  label="子网掩码"
                  required
                  outlined
                  disabled
              ></v-text-field>
            </v-card-text>
          </v-card>
        </v-hover>
      </v-col>

      <v-spacer></v-spacer>

      <v-col sm="4">
        <v-hover v-slot="{ hover }">
          <v-card :elevation="hover ? 12 : 4">
            <v-card-title>Router1</v-card-title>
            <v-card-text>
              <v-text-field
                  v-model="serial0_r1"
                  label="Serial0/0/0"
                  required
                  outlined
                  disabled
              ></v-text-field>
              <v-text-field
                  v-model="serial1_r1"
                  label="Serial0/0/1"
                  required
                  outlined
                  disabled
              ></v-text-field>
              <v-text-field
                  v-model="mask_r1"
                  label="子网掩码"
                  required
                  outlined
                  disabled
              ></v-text-field>
            </v-card-text>
          </v-card>
        </v-hover>
      </v-col>

      <v-spacer></v-spacer>

      <v-col sm="4">
        <v-hover v-slot="{ hover }">
          <v-card :elevation="hover ? 12 : 4">
            <v-card-title>Router2</v-card-title>
            <v-card-text>
              <v-text-field
                  v-model="serial0_r2"
                  label="Serial0/0/0"
                  required
                  outlined
                  disabled
              ></v-text-field>
              <v-text-field
                  v-model="serial1_r2"
                  label="Serial0/0/1"
                  required
                  outlined
                  disabled
              ></v-text-field>
              <v-text-field
                  v-model="mask_r2"
                  label="子网掩码"
                  required
                  outlined
                  disabled
              ></v-text-field>
            </v-card-text>
          </v-card>
        </v-hover>
      </v-col>
    </v-row>

    <v-row class="text-center">
      <v-col>
        <v-btn
            elevation="4"
            color="primary"
            @click="init"
        >
          初始化串口
        </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  name: "Serial",
  data() {
    return {
      // 消息条
      show_snackbar: false,
      icon: 'mdi-minus-circle',
      snackbar_text: '网络连接失败',
      color: 'warning',
      // 串行接口Serial0/0/0
      serial0_r0: '172.17.0.1',
      serial0_r1: '172.17.0.2',
      serial0_r2: '172.18.0.2',
      // 串行接口Serial0/0/1
      serial1_r0: '-',
      serial1_r1: '172.18.0.1',
      serial1_r2: '-',
      // 串行接口子网掩码
      mask_r0: '255.255.0.0',
      mask_r1: '255.255.0.0',
      mask_r2: '255.255.0.0'
    }
  },
  methods: {
    // 弹出消息条
    tip() {
      this.show_snackbar = true
      //保存组件对象
      let _this = this
      setTimeout(function () {
        // 重置消息条信息
        _this.snackbar_text = "网络连接失败"
        _this.icon = 'mdi-minus-circle'
        _this.color = 'warning'
      }, 6000)
    },
    // 初始化串行接口
    init() {
      const url = 'http://127.0.0.1:5000/init'
      let data = {
        r0: {
          serial_ip: [this.serial0_r0, this.serial1_r0],
          mask: this.mask_r0
        },
        r1: {
          serial_ip: [this.serial0_r1, this.serial1_r1],
          mask: this.mask_r1
        },
        r2: {
          serial_ip: [this.serial0_r2, this.serial1_r2],
          mask: this.mask_r2
        }
      }
      axios({
        url: url,
        method: 'post',
        data: data
      }).then(res => {
        console.log(res)
        if (res.data.state) {
          // 设置消息条
          this.snackbar_text = res.data.msg
          this.icon = 'mdi-checkbox-marked-circle'
          this.color = 'success'
        } else {
          // 设置消息条
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
