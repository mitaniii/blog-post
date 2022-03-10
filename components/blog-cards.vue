<template>
  <v-card>
    <v-subheader>{{ blog.title }}</v-subheader>
    <v-list-item>{{ blog.content }}</v-list-item>
    <v-list-item>{{ blog.timestamp }}</v-list-item>
    <v-btn @click="remove(blog.id)">
      <v-icon small>
        mdi-delete
      </v-icon>
    </v-btn>
    <v-btn @click="isUpdate = !isUpdate">
      <v-icon small>
        mdi-update
      </v-icon>
    </v-btn>
    <blog-update v-show="isUpdate" :blog="blog" />
  </v-card>
</template>

<script>
export default {
  name: 'BlogCards',
  props: {
    blog: {
      type: Object, default: null
    }
  },
  data () {
    return {
      isUpdate: false
    }
  }
}
</script>

<style>

</style>
