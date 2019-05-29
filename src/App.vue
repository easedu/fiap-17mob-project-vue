<template>
  <v-app id="inspire" >
    <v-toolbar color="red" dense fixed app class="nome">
      <v-toolbar-title class="mr-5 align-center">
        <span class="title">EAS GIT REPOSITORIES</span>
      </v-toolbar-title>
    </v-toolbar>
    <v-content>
      <v-container fluid id="app">
        <router-view/>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
  export default {
    data: () => ({
      drawer: null
    }),
    props: {
      source: String
    }
  }
</script>
      
<style>

.title {
  color: #fff;
}
</style>