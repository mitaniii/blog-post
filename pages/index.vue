<template>
  <v-app>
    <v-navigation-drawer v-model="drawer" app />
    <v-app-bar app>
      <v-toolbar-title class="mx-auto">
        <h1>
          Blog-Post-App
        </h1>
      </v-toolbar-title>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
    </v-app-bar>
    <v-main>
      <v-container>
        <Blog-List :blogs="blogs" />
        <Blog-Form @add="addEvent" />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'IndexPage'
  // data () {
  //   return {
  //     drawer: false,
  //     blogs: []
  //   }
  // },
  // methods: {
  //   addEvent (blog) {
  //     this.blogs.push(blog)
  //   }
  // }
}
</script>
