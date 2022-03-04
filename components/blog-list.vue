<template>
  <v-container>
    <v-list v-for="blog in blogs" :key="blog.title">
      <v-card>
        <v-subheader>{{ blog.title }}</v-subheader>
        <v-list-item>{{ blog.content }}</v-list-item>
      </v-card>
    </v-list>
  </v-container>
</template>

<script>
export default {
  name: 'BlogList',
  props: {
    blogs: { type: Array, default: null }
  }
}
</script>

<style>

</style>
