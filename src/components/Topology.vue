<template>
  <v-container>
    <!-- 标题 -->
    <h3>网络拓扑</h3>
    <p style="white-space: pre-line"></p>
    <!-- 拓补图 -->
    <v-row>
      <v-col>
        <v-hover v-slot="{ hover }">
          <v-card :elevation="hover ? 12 : 4">
            <v-img
                src="../assets/Topography.png"
                contain
                max-height="320"
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
