<template>
  <v-container>
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
            :loading="loader_r0 || loader_r1 || loader_r2"
            @click="config_all"
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
            :loading="loader_info"
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
            dense
            dark
            background-color="#212121"
            height="500"
            no-resize
            class="console"
        >
        </v-textarea>
      </v-col>
    </v-row>

    <!-- 清空按钮 -->
    <v-row class="text-center">
      <v-col sm="10"></v-col>
      <v-col sm="2">
        <v-btn
            large
            elevation="4"
            color="primary"
            @click="msg='## Telnet Client ##\n'"
        >
          清空控制台
        </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios'
import Serial from "@/components/Serial"

export default {
  name: "Config",
  data() {
    return {
      // 路由协议列表
      protocol_selected: 'RIP',
      protocol_list: ['RIP', 'OSPF'],
      // 设备列表
      dev_selected: 'Router1',
      dev_list: ['Switch2', 'Router0', 'Router1', 'Router2'],
      // 加载器
      loader_r0: false,
      loader_r1: false,
      loader_r2: false,
      loader_info: false,
      // 控制台信息
      msg: '## Telnet Client ##\n'
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
      let set_loader = "this.loader_" + dev_no + " = " + state
      eval(set_loader)
    },
    // 配置协议
    config(dev_no, dev_data) {
      // 设置加载器
      let set_loader = "this.loader_" + dev_no + " = true"
      eval(set_loader)
      // 配置协议
      let url = 'http://127.0.0.1:5000/config/' + this.protocol_selected.toLowerCase()
      let data = {
        dev_no: dev_no,
        dev_data: dev_data
      }
      // 调用接口
      axios({
        method: 'post',
        url: url,
        data: data
      }).then(res => {
        console.log(res)
        if (res.data.state) {
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
    // 一键配置协议
    config_all() {
      axios.all([this.config("r0", Serial.data().r0),
        this.config("r1", Serial.data().r1),
        this.config("r2", Serial.data().r2)
      ]).then(axios.spread(function (res) {
        console.log(res);
      })).catch(err => {
        console.log(err)
      })
    },
    // 查看信息
    info() {
      this.setLoader('info', true) // 设置加载器
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
        if (res.data.state) {
          this.showMessage('mdi-checkbox-marked-circle', res.data.msg, 'success')
          this.setLoader('info', false)
        } else {
          this.showMessage('mdi-cancel', res.data.msg, 'error')
          this.setLoader('info', false)
        }
        // 输出信息到控制台
        let result = res.data
        this.msg += '# IP Route\n' + result.info.route + '\n# IP Protocols\n' + result.info.protocol + '\n'
      }).catch(err => {
        console.log(err)
        this.showMessage('mdi-minus-circle', '网络连接失败', 'warning')
        this.setLoader('info', false)
      })
    }
  }
}
</script>

<style scoped lang="less">
.console {
  font-family: Consolas, sans-serif;
  font-size: 16px;

  ::selection {
    color: black;
    background: white;
  }
}
</style>
