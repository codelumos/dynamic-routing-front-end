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
        路由配置
      </p>
    </v-row>

    <!-- 路由协议及设备选择 -->
    <v-row>
      <v-col sm="4">
        <v-select
            v-model="protocol_selected"
            :items="protocol_list"
            label="路由协议"
            outlined
        ></v-select>
      </v-col>
      <v-col sm="2">
        <v-btn
            elevation="4"
            color="primary"
            @click="config"
        >
          配置路由
        </v-btn>
      </v-col>

      <v-col sm="4">
        <v-select
            v-model="dev_selected"
            :items="dev_list"
            label="设备选择"
            outlined
        ></v-select>
      </v-col>
      <v-col sm="2">
        <v-btn
            elevation="4"
            color="primary"
            @click="info"
        >
          查看路由
        </v-btn>
      </v-col>
    </v-row>

    <!-- 控制台 -->
    <v-row>
      <p class="title font-weight-bold mb-3">
        控制台
      </p>
    </v-row>

    <v-row>
      <v-col cols="12">
        <v-textarea
            v-model="msg"
            outlined
            readonly
            dark
            background-color="#212121"
        >
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
      color: 'warning',
      // 路由协议列表
      protocol_selected: 'RIP',
      protocol_list: ['RIP', 'OSPF', 'BGP'],
      // 设备列表
      dev_selected: 'Switch2',
      dev_list: ['Switch2', 'Router0', 'Router1', 'Router2'],
      // 控制台信息
      msg: 'Dynamic Router Telnet\nRouter>'
    }
  },
  methods: {
    // 配置协议
    config() {
      //初始化信息条
      this.snackbar_text = '配置失败'
      this.icon = 'mdi-cancel'
      this.color = 'error'

      let url = 'http://127.0.0.1:5000/config/' + this.protocol_selected.toLowerCase()
      // 调用接口
      axios({
        method: 'post',
        url: url
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
    },
    info() {
      let data = {
        dev_no: this.dev_selected[0].toLowerCase() + this.dev_selected[this.dev_selected.length - 1]
      }
      let url = 'http://127.0.0.1:5000/info'
      // 调用接口
      axios({
        method: 'post',
        url: url,
        data: data
      }).then(res => {
        console.log(res)
        // 输出信息到控制台
        let result = res.data
        this.msg += result.route + '\n' + result.protocol + '\n'
      }).catch(err => {
        console.log(err)
      })
    }
  }
}
</script>
