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
    <BlogUpdate v-show="isUpdate" :blog="blog" />
  </v-card>
</template>

<script>
import BlogUpdate from '../pages/blog-update'

export default {
  name: 'BlogCards',
  props: {
    blog: {
      type: Object, default: null
    }
  },
  components: {
    BlogUpdate
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
