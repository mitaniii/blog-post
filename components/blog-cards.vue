<template>
  <v-card>
    <v-subheader>{{ blog.userDisplayName }}さんの投稿</v-subheader>
    <v-subheader>タイトル：{{ blog.title }}</v-subheader>
    <v-list-item>記事：{{ blog.content }}</v-list-item>
    <v-list-item>更新：{{ blog.timestamp.toDate() }}</v-list-item>
    <div v-for="comment in comments" :key="comment.id">
      <div v-if="comment.id === blog.id">
        <v-subheader>{{ comment.comment }}</v-subheader>
      </div>
    </div>
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
      <v-btn @click="isComment = !isComment">
        <v-icon>
          mdi-cat
        </v-icon>
      </v-btn>
      <CommentForm v-show="isComment" :blog="blog" @close="closeCommentForm" />
    </div>
    <div v-else />
  </v-card>
</template>

<script>
import { collection, deleteDoc, doc, onSnapshot } from '@firebase/firestore'
import { onAuthStateChanged } from '@firebase/auth'
import { auth, db } from '../plugins/firebase'
import BlogUpdate from '../pages/blog-update'
import CommentForm from '../pages/comment-form'
const usersCollectionRef = collection(db, 'comments')

export default {
  name: 'BlogCards',
  components: {
    BlogUpdate,
    CommentForm
  },
  props: {
    blog: {
      type: Object, default: null
    }
  },
  data () {
    return {
      isUpdate: false,
      isComment: false,
      user: '',
      comments: []
    }
  },
  mounted () {
    onAuthStateChanged(auth, (user) => {
      this.user = user
    })
    onSnapshot(usersCollectionRef, (querySnapshot) => {
      this.comments = querySnapshot.docs.map(doc => ({ ...doc.data(), id: doc.id }))
      this.comments.sort((a, b) => b.timestamp - a.timestamp)
    })
  },
  methods: {
    remove (id) {
      const userDocumentRef = doc(db, 'blogs', id)
      deleteDoc(userDocumentRef)
    },
    closeUpdateForm () {
      this.isUpdate = false
    },
    closeCommentForm () {
      this.isComment = false
    }
  }
}
</script>

<style>

</style>
