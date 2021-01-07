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
                  v-model="r0.serial0"
                  label="Serial0/0/0"
                  required
                  outlined
                  disabled
              ></v-text-field>
              <v-text-field
                  v-model="r0.serial1"
                  label="Serial0/0/1"
                  required
                  outlined
                  disabled
              ></v-text-field>
              <v-text-field
                  v-model="r0.mask"
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
                  v-model="r1.serial0"
                  label="Serial0/0/0"
                  required
                  outlined
                  disabled
              ></v-text-field>
              <v-text-field
                  v-model="r1.serial1"
                  label="Serial0/0/1"
                  required
                  outlined
                  disabled
              ></v-text-field>
              <v-text-field
                  v-model="r1.mask"
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
                  v-model="r2.serial0"
                  label="Serial0/0/0"
                  required
                  outlined
                  disabled
              ></v-text-field>
              <v-text-field
                  v-model="r2.serial1"
                  label="Serial0/0/1"
                  required
                  outlined
                  disabled
              ></v-text-field>
              <v-text-field
                  v-model="r2.mask"
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
            :loading="loader"
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
      r0: {
        serial0: '172.17.0.1', // 串行接口Serial0/0/0
        serial1: '-', // 串行接口Serial0/0/1
        mask: '255.255.0.0' // 串行接口子网掩码
      },
      r1: {
        serial0: '172.17.0.2', // 串行接口Serial0/0/0
        serial1: '172.18.0.1', // 串行接口Serial0/0/1
        mask: '255.255.0.0' // 串行接口子网掩码
      },
      r2: {
        serial0: '172.18.0.2', // 串行接口Serial0/0/0
        serial1: '-', // 串行接口Serial0/0/1
        mask: '255.255.0.0' // 串行接口子网掩码
      },
      loader: false // 加载器
    }
  },
  methods: {
    // 弹出消息条
    showMessage(icon, msg, color) {
      // 通过触发showSnackbar事件并传递消息参数，从而调用全局Snackbar
      this.$eventBus.$emit('showSnackbar', {
        id: new Date().getTime(), // id用于设置Snackbar在v-for循环中的key属性，避免排序混乱的问题
        content: {icon, msg, color},
      })
    },
    // 关闭加载器
    closeLoader() {
      this.loader = false
    },
    // 初始化串行接口
    init() {
      this.loader = true // 设置加载器
      const url = 'http://127.0.0.1:5000/init'
      let data = {
        r0: {
          serial_ip: [this.r0.serial0, this.r0.serial1],
          mask: this.r0.mask
        },
        r1: {
          serial_ip: [this.r1.serial0, this.r1.serial1],
          mask: this.r1.mask
        },
        r2: {
          serial_ip: [this.r2.serial0, this.r2.serial1],
          mask: this.r2.mask
        }
      }
      axios({
        url: url,
        method: 'post',
        data: data
      }).then(res => {
        console.log(res)
        if (res.data.state) {
          this.showMessage('mdi-checkbox-marked-circle', res.data.msg, 'success')
          this.closeLoader()
        } else {
          this.showMessage('mdi-cancel', res.data.msg, 'error')
          this.closeLoader()
        }
      }).catch(err => {
        console.log(err)
        this.showMessage('mdi-minus-circle', '网络连接失败', 'warning')
        this.closeLoader()
      })
    }
  }
}
</script>
