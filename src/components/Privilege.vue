<template>
  <v-container>
    <!-- 标题 -->
    <v-row>
      <p class="title font-weight-bold mb-3">
        特权密码
      </p>
    </v-row>

    <!-- 配置卡片 -->
    <v-row class="text-center" v-show="!unify.enable">
      <v-col sm="3">
        <v-card elevation="4">
          <v-card-title>Switch2</v-card-title>
          <v-card-text>
            <v-text-field
                v-model="s2.pwd"
                label="特权密码"
                required
                outlined
                :append-icon="s2.show ? 'mdi-eye' : 'mdi-eye-off'"
                :type="s2.show ? 'text' : 'password'"
                @click:append="s2.show = !s2.show"
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
                v-model="r0.pwd"
                label="特权密码"
                required
                outlined
                :append-icon="r0.show ? 'mdi-eye' : 'mdi-eye-off'"
                :type="r0.show ? 'text' : 'password'"
                @click:append="r0.show = !r0.show"
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
                v-model="r1.pwd"
                label="特权密码"
                required
                outlined
                :append-icon="r1.show ? 'mdi-eye' : 'mdi-eye-off'"
                :type="r1.show ? 'text' : 'password'"
                @click:append="r1.show = !r1.show"
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
                v-model="r2.pwd"
                label="特权密码"
                required
                outlined
                :append-icon="r2.show ? 'mdi-eye' : 'mdi-eye-off'"
                :type="r2.show ? 'text' : 'password'"
                @click:append="r2.show = !r2.show"
            ></v-text-field>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>

    <!-- 统一密码选项 -->
    <p style="white-space: pre-line" v-show="!unify.enable"></p>
    <v-row>
      <v-col sm="3">
        <v-switch
            v-model="unify.enable"
            :label="`使用统一密码`"
        ></v-switch>
      </v-col>
      <v-col sm="6">
        <v-text-field
            v-show="unify.enable"
            v-model="unify.pwd"
            label="统一特权密码"
            required
            outlined
            :append-icon="unify.show ? 'mdi-eye' : 'mdi-eye-off'"
            :type="unify.show ? 'text' : 'password'"
            @click:append="unify.show = !unify.show"
        ></v-text-field>
      </v-col>
      <v-col sm="3">
        <v-btn
            elevation="4"
            color="primary"
            :loading="unify.loader"
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
      s2: {
        pwd: '', // 特权密码
        show: false, // 密码可见性
      },
      r0: {
        pwd: '', // 特权密码
        show: false, // 密码可见性
      },
      r1: {
        pwd: '', // 特权密码
        show: false, // 密码可见性
      },
      r2: {
        pwd: '', // 特权密码
        show: false, // 密码可见性
      },
      unify: {
        enable: true, // 使用统一密码
        pwd: '', // 统一特权密码
        show: false, // 密码可见性
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
    // 关闭加载器
    closeLoader() {
      this.unify.loader = false
    },
    // 进入特权模式
    enable() {
      // 检查密码是否为空
      if ((this.unify.enable && this.unify.pwd === '') || (!this.unify.enable && (this.r0.pwd === '' || this.r1.pwd === '' || this.r2.pwd === '' || this.s2.pwd === ''))) {
        this.showMessage('mdi-alert-circle', '密码不能为空', 'warning')
        this.closeLoader()
        return
      }
      // 设置加载器
      this.unify.loader = true
      const url = 'http://127.0.0.1:5000/enable'
      // 进入特权模式
      let data = {
        pwd_r0: this.unify.enable ? this.unify.pwd : this.r0.pwd,
        pwd_r1: this.unify.enable ? this.unify.pwd : this.r1.pwd,
        pwd_r2: this.unify.enable ? this.unify.pwd : this.r2.pwd
      }
      axios({
        method: 'post',
        url: url,
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
