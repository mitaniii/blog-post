<template>
  <v-app>
    <v-main>
      <v-container>
        <v-form @submit.prevent="addComment">
          <div v-if="user">
            <v-row>
              <v-col cols="12" sm="10">
                <v-textarea v-model="comment" label="Comment" />
              </v-col>
              <v-col cols="12" sm="10">
                <v-btn type="submit" color="primary">
                  コメントする
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
const usersCollectionRef = collection(db, 'comments')

export default {
  name: 'CommentForm',
  props: {
    blog: {
      type: Object, default: null
    }
  },
  data () {
    return {
      comment: '',
      user: ''
    }
  },
  mounted () {
    onAuthStateChanged(auth, (user) => {
      this.user = user
    })
  },
  methods: {
    addComment () {
      if (this.comment.trim()) {
        addDoc(usersCollectionRef, {
          comment: this.comment,
          blogId: this.blog.id,
          userUid: this.user.uid,
          timestamp: serverTimestamp()
        }).then(() => {
          this.comment = ''
          this.$emit('close')
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
