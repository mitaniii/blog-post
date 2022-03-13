<template>
  <v-app>
    <v-main>
      <v-container>
        <v-form @submit.prevent="addBlog">
          <div v-if="user">
            <v-row>
              <v-col cols="12" sm="10">
                <v-text-field v-model="title" label="タイトル" />
              </v-col>
              <v-col cols="12" sm="10">
                <v-textarea v-model="content" label="記事" />
              </v-col>
              <v-col cols="12" sm="10">
                <v-btn type="submit" color="#00E676" dark>
                  投稿
                </v-btn>
                <v-btn color="#00E676" to="/" dark>
                  一覧に戻る
                </v-btn>
              </v-col>
              <v-col cols="12">
                {{ message }}
              </v-col>
            </v-row>
          </div>
          <div v-else>
            <nuxt-link to="/login">
              ログインしてください
            </nuxt-link>
          </div>
        </v-form>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import { onAuthStateChanged, signOut } from '@firebase/auth'
import { addDoc, collection, serverTimestamp } from '@firebase/firestore'
import { auth, db } from '../plugins/firebase'
const usersCollectionRef = collection(db, 'blogs')

export default {
  name: 'BlogForm',
  data () {
    return {
      title: '',
      content: '',
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
          userUid: this.user.uid,
          userDisplayName: this.user.displayName,
          timestamp: serverTimestamp()
        }).then(() => {
          this.title = ''
          this.content = ''
          this.$router.push('/')
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
