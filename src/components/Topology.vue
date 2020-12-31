<template>
  <v-container>
    <!-- 标题 -->
    <v-row>
      <p class="title font-weight-bold mb-3">
        网络拓补
      </p>
    </v-row>

    <!-- 拓补图 -->
    <v-row>
      <v-col>
        <v-hover v-slot="{ hover }">
          <v-card :elevation="hover ? 12 : 4">
            <v-img
                :src="require('../assets/Topography.png')"
                class="my-3"
                contain
                height="350"
            />
          </v-card>
        </v-hover>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "Topology"
}
</script>
