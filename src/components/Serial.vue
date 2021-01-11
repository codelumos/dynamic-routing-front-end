<template>
  <v-container>
    <!-- 标题 -->
    <v-row>
      <p class="title font-weight-bold mb-3">
        串行接口配置
      </p>
    </v-row>

    <v-card flat>
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
              :disabled="r0.state && r1.state && r2.state"
              :loading="r0.loader && r1.loader && r2.loader"
              @click="init_all"
          >
            初始化串口
          </v-btn>
        </v-col>
      </v-row>

      <!-- 遮罩层 -->
      <v-overlay
          absolute
          :z-index=0
          :value="r0.state && r1.state && r2.state"
      >
        <v-btn color="success">
          串行接口已初始化
        </v-btn>
      </v-overlay>
    </v-card>
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
        mask: '255.255.0.0', // 串行接口子网掩码
        state: false, // 配置状态(true:已初始化, false:未初始化)
        loader: false // 加载器
      },
      r1: {
        serial0: '172.17.0.2', // 串行接口Serial0/0/0
        serial1: '172.18.0.1', // 串行接口Serial0/0/1
        mask: '255.255.0.0', // 串行接口子网掩码
        state: false, // 配置状态(true:已初始化, false:未初始化)
        loader: false // 加载器
      },
      r2: {
        serial0: '172.18.0.2', // 串行接口Serial0/0/0
        serial1: '-', // 串行接口Serial0/0/1
        mask: '255.255.0.0', // 串行接口子网掩码
        state: false, // 配置状态(true:已初始化, false:未初始化)
        loader: false // 加载器
      }
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
    // 设置加载器
    setLoader(dev_no, state) {
      let set_loader = "this." + dev_no + ".loader = " + state
      eval(set_loader)
    },
    // 设置配置状态
    setState(dev_no, state) {
      let set_state = "this." + dev_no + ".state = " + state
      eval(set_state)
    },
    // 初始化串行接口
    init(dev_no, ip_list, mask) {
      // 检查设备状态
      let state_check = "this." + dev_no + ".state === true" // 已登录
      let waiting_check = "this." + dev_no + ".loader === true" // 登陆中
      if (eval(state_check) || eval(waiting_check)) {
        return
      }
      // 设置加载器
      this.setLoader(dev_no, true)
      // 初始化串行接口
      const url = 'http://127.0.0.1:5000/init'
      let data = {
        dev_no: dev_no,
        ip_list: ip_list,
        mask: mask
      }
      axios({
        url: url,
        method: 'post',
        data: data
      }).then(res => {
        console.log(res)
        if (res.data.state) {
          // 将状态设为已初始化
          this.setState(dev_no, true)
          this.showMessage('mdi-checkbox-marked-circle', res.data.msg, 'success')
          this.setLoader(dev_no, false)
        } else {
          this.showMessage('mdi-cancel', res.data.msg, 'error')
          this.setLoader(dev_no, false)
        }
      }).catch(err => {
        console.log(err)
        this.showMessage('mdi-minus-circle', '网络连接失败', 'warning')
        this.setLoader(dev_no, false)
      })
    },
    // 一键初始化串行接口
    init_all() {
      axios.all([this.init("r0", [this.r0.serial0, this.r0.serial1], this.r0.mask),
        this.init("r1", [this.r1.serial0, this.r1.serial1], this.r1.mask),
        this.init("r2", [this.r2.serial0, this.r2.serial1], this.r2.mask)
      ]).then(axios.spread(function (res) {
        console.log(res);
      })).catch(err => {
        console.log(err)
      })
    }
  }
}
</script>
