<template>
  <v-container>
    <v-row class="text-center">
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

      <v-spacer></v-spacer>

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

      <v-spacer></v-spacer>

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

      <v-spacer></v-spacer>

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
    </v-row>
  </v-container>

</template>

<script>
export default {
  name: "Config",
  data() {
    return {
      show_r0_2: false,
      show_r1_2: false,
      show_r2_2: false,
      show_s2_2: false,
      pwd_enable_r0: '',
      pwd_enable_r1: '',
      pwd_enable_r2: '',
      pwd_enable_s2: ''
    }
  }
}
</script>
