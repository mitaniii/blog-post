<template>
  <v-card>
    <v-subheader>{{ blog.userDisplayName }}さんの投稿</v-subheader>
    <v-subheader>タイトル：{{ blog.title }}</v-subheader>
    <v-list-item>記事：{{ blog.content }}</v-list-item>
    <v-list-item>更新：{{ blog.timestamp.toDate() }}</v-list-item>
    <div v-if="user && blog.userUid === user.uid">
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
      <BlogUpdate v-show="isUpdate" :blog="blog" @close="closeUpdateForm" />
    </div>
    <div v-else />
  </v-card>
</template>

<script>
import { deleteDoc, doc } from '@firebase/firestore'
import { onAuthStateChanged } from '@firebase/auth'
import { auth, db } from '../plugins/firebase'
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
      isUpdate: false,
      user: ''
    }
  },
  mounted () {
    onAuthStateChanged(auth, (user) => {
      this.user = user
    })
  },
  methods: {
    remove (id) {
      const userDocumentRef = doc(db, 'blogs', id)
      deleteDoc(userDocumentRef)
    },
    closeUpdateForm () {
      this.isUpdate = false
    }
  }
}
</script>

<style>

</style>
