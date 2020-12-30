<template>
  <v-container>
    <v-row>
      <p class="title font-weight-bold mb-3">
        路由配置
      </p>
    </v-row>

    <p style="white-space: pre-line"></p>

    <v-row class="text-center">
      <v-col sm="3">
        <v-card elevation="4">
          <v-card-title>Router0</v-card-title>
          <v-card-text>
            <v-text-field
                v-model="pwd_enable_r0"
                label="特权密码"
                required
                outlined
                :append-icon="show_r0_2 ? 'mdi-eye' : 'mdi-eye-off'"
                :type="show_r0_2 ? 'text' : 'password'"
                @click:append="show_r0_2 = !show_r0_2"
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
                v-model="pwd_enable_r1"
                label="特权密码"
                required
                outlined
                :append-icon="show_r1_2 ? 'mdi-eye' : 'mdi-eye-off'"
                :type="show_r1_2 ? 'text' : 'password'"
                @click:append="show_r1_2 = !show_r1_2"
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
                v-model="pwd_enable_r2"
                label="特权密码"
                required
                outlined
                :append-icon="show_r2_2 ? 'mdi-eye' : 'mdi-eye-off'"
                :type="show_r2_2 ? 'text' : 'password'"
                @click:append="show_r2_2 = !show_r2_2"
            ></v-text-field>
          </v-card-text>
        </v-card>
      </v-col>

      <v-spacer></v-spacer>

      <v-col sm="3">
        <v-card elevation="4">
          <v-card-title>Switch2</v-card-title>
          <v-card-text>
            <v-text-field
                v-model="pwd_enable_s2"
                label="特权密码"
                required
                outlined
                :append-icon="show_s2_2 ? 'mdi-eye' : 'mdi-eye-off'"
                :type="show_s2_2 ? 'text' : 'password'"
                @click:append="show_s2_2 = !show_s2_2"
            ></v-text-field>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>

    <p style="white-space: pre-line"></p>

    <v-row>
      <v-col sm="10">
        <v-select
            :items="['RIP', 'OSPF']"
            label="路由协议"
            outlined
        ></v-select>
      </v-col>
      <v-col sm="2">
        <v-btn elevation="2" color="primary">
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
export default {
  name: "Config",
  data() {
    return {
      pwd_enable_r0: '',
      show_r0_2: false,
      pwd_enable_r1: '',
      show_r1_2: false,
      pwd_enable_r2: '',
      show_r2_2: false,
      pwd_enable_s2: '',
      show_s2_2: false
    }
  }
}
</script>
