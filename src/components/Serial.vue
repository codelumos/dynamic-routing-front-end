<template>
  <v-container>
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
    showMessage(icon, msg, color) {
      // 通过触发 showSnackbar 事件并传递消息参数，从而调用全局 Snackbar
      this.$eventBus.$emit('showSnackbar', {
        id: new Date().getTime(), // id 用于设置 Snackbar 在 v-for 循环中的 key 属性，避免排序混乱的问题
        content: {icon, msg, color},
      })
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
          // 弹出消息条
          this.showMessage('mdi-checkbox-marked-circle', res.data.msg, 'success')
        } else {
          // 弹出消息条
          this.showMessage('mdi-cancel', res.data.msg, 'error')
        }
      }).catch(err => {
        console.log(err)
        // 弹出消息条
        this.showMessage('mdi-minus-circle', '网络连接失败', 'warning')
      })
    }
  }
}
</script>
