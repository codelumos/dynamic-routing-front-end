<template>
  <v-container>
    <v-row>
      <p class="title font-weight-bold mb-3">
        网络拓补
      </p>
    </v-row>

    <v-row>
      <v-col>
        <v-card>
          <v-img
              :src="require('../assets/Topography.png')"
              class="my-3"
              contain
              height="350"
          />
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "Topology"
}
</script>
