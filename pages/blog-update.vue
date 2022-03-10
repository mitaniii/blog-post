<template>
  <v-container>
    <v-form @submit.prevent="addBlog">
      <div v-if="user">
        <p>{{ user.displayName }}</p>
        <v-btn @click="logout">
          ログアウト
        </v-btn>
        <v-row>
          <v-col cols="12" sm="10">
            <v-text-field v-model="title" label="Title" />
          </v-col>
          <v-col cols="12" sm="10">
            <v-textarea v-model="content" label="Content" />
          </v-col>
          <v-col cols="12" sm="10">
            <v-btn type="submit" color="primary">
              Submit
            </v-btn>
          </v-col>
          <v-col cols="12">
            {{ message }}
          </v-col>
        </v-row>
      </div>
      <div v-else />
    </v-form>
  </v-container>
</template>

<script>
import { onAuthStateChanged, signOut } from '@firebase/auth'
import { addDoc, collection, serverTimestamp } from '@firebase/firestore'
import { auth, db } from '../plugins/firebase'
const usersCollectionRef = collection(db, 'blogs')

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
    addBlog () {
      if (this.title.trim() && this.content.trim()) {
        addDoc(usersCollectionRef, {
          title: this.title,
          content: this.content,
          timestamp: serverTimestamp()
        }).then(() => {
          this.title = ''
          this.content = ''
        })
      }
    },
    logout () {
      signOut(auth).then(() => (this.user = null))
    }
  }
}
</script>

<style>

</style>
