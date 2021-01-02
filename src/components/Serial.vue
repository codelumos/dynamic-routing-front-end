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
    </v-row>

    <v-row class="text-center">
      <v-col>
        <v-btn
            elevation="4"
            color="primary"
        >
          初始化串口
        </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "Serial",
  data() {
    return {
      // 串行接口Serial0/0/0
      serial0_r0: '127.17.0.1',
      serial0_r1: '127.17.0.0',
      serial0_r2: '127.18.0.2',
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
}
</script>
