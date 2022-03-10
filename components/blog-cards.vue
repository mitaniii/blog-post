<template>
  <v-card>
    <v-subheader>{{ blog.title }}</v-subheader>
    <v-list-item>{{ blog.content }}</v-list-item>
    <v-list-item>{{ blog.timestamp.toDate() }}</v-list-item>
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
import { deleteDoc, doc } from '@firebase/firestore'
import { db } from '../plugins/firebase'
import BlogUpdate from '../pages/blog-update'

export default {
  name: 'BlogCards',
  components: {
    BlogUpdate
  },
  props: {
    blog: {
      type: Object, default: null
    }
  },
  data () {
    return {
      isUpdate: false
    }
  },
  methods: {
    remove (id) {
      const userDocumentRef = doc(db, 'blogs', id)
      deleteDoc(userDocumentRef)
    }
  }
}
</script>

<style>

</style>
