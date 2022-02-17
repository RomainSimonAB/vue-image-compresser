<template>
  <v-app>
    <v-app-bar
      fixed
      app
      elevation="0"
      dark
    >
      <v-toolbar-title>
        Image Reducer
      </v-toolbar-title>
      <v-spacer />
    </v-app-bar>
    <v-main>
      <v-container class="py-8">
        <Nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data () {
    return {

    }
  }
}
</script>
