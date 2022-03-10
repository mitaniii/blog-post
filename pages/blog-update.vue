<template>
  <v-container>
    <v-form @submit.prevent="updateBlog">
      <div v-if="user">
        - - - - - - - - - - - - - - ブログ更新- - - - - - - - - - - - - -
        <v-row>
          <v-col cols="12" sm="10">
            <v-text-field v-model="title" label="Title" />
          </v-col>
          <v-col cols="12" sm="10">
            <v-textarea v-model="content" label="Content" />
          </v-col>
          <v-col cols="12" sm="10">
            <v-btn type="submit" color="primary">
              更新
            </v-btn>
          </v-col>
        </v-row>
      </div>
      <div v-else />
    </v-form>
  </v-container>
</template>

<script>
import { onAuthStateChanged } from '@firebase/auth'
import { doc, serverTimestamp, updateDoc } from '@firebase/firestore'
import { auth, db } from '../plugins/firebase'

export default {
  name: 'BlogUpdate',
  props: {
    blog: {
      type: Object, default: null
    }
  },
  data () {
    return {
      title: this.blog.title,
      content: this.blog.content,
      user: ''
    }
  },
  mounted () {
    onAuthStateChanged(auth, (user) => {
      this.user = user
    })
  },
  methods: {
    updateBlog () {
      if (this.title.trim() && this.content.trim()) {
        const userDocumentRef = doc(db, 'blogs', this.blog.id)
        updateDoc(userDocumentRef, {
          title: this.title,
          content: this.content,
          timestamp: serverTimestamp()
        }).then(() => {
          this.title = ''
          this.content = ''
          this.$router.push('/')
        })
      }
    }
  }
}
</script>

<style>

</style>
