<template>
  <v-card class="my-5">
    <v-card-title class="green accent-3">
      {{ blog.userDisplayName }}さんの投稿
      <v-spacer />
      <v-card>
        <div v-show="isUserContent">
          <div v-if="user">
            <v-card-text>
              <v-chip-group active-class="green accent-3 white--text">
                <v-chip @click="isComment = !isComment">
                  <v-icon>
                    mdi-comment-processing-outline
                  </v-icon>
                </v-chip>
                <div v-if="user && blog.userUid === user.uid">
                  <v-chip @click="remove(blog.id)">
                    <v-icon>
                      mdi-delete
                    </v-icon>
                  </v-chip>
                  <v-chip @click="isUpdate = !isUpdate">
                    <v-icon>
                      mdi-update
                    </v-icon>
                  </v-chip>
                </div>
                <div v-else />
              </v-chip-group>
            </v-card-text>
          </div>
        </div>
      </v-card>
      <v-spacer />
      <v-icon @click="isUserContent = !isUserContent">
        mdi-format-list-bulleted
      </v-icon>
    </v-card-title>
    <v-list color="green lighten-4">
      <v-card-title class="text-subtitle-1">
        タイトル：{{ blog.title }}
      </v-card-title>
    </v-list>
    <v-list class="overflow-y-auto" height="150">
      <v-card-title>
        記事：{{ blog.content }}
      </v-card-title>
    </v-list>
    <v-list-item class="text-caption">
      {{ blog.timestamp.toDate() }}
    </v-list-item>
    <div>コメントを見る</div>
    <v-icon @click="ischeck = !ischeck">
      mdi-unfold-more-horizontal
    </v-icon>
    <div v-show="ischeck" class="overflow-x-auto" width="150">
      <div v-for="comment in displayComments" :key="comment.index">
        <div v-if="comment.blogId === blog.id">
          <v-subheader>
            <v-col cols="12">
              <v-list-item>
                {{ comment.comment }}
                <v-spacer />
                <div v-if="user && comment.userUid === user.uid">
                  <v-icon small @click="commentRemove(comment.id)">
                    mdi-delete
                  </v-icon>
                </div>
              </v-list-item>
            </v-col>
          </v-subheader>
        </div>
      </div>
      <v-pagination v-model="page" :length="length" @input="pageChange" />
    </div>
    <CommentForm v-show="isComment" :blog="blog" />
    <BlogUpdate v-show="isUpdate" :blog="blog" />
    <!-- <CommentForm v-show="isComment" :blog="blog" @close="closeCommentForm" />
    <BlogUpdate v-show="isUpdate" :blog="blog" @close="closeUpdateForm" /> -->
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
      ischeck: false,
      isUserContent: false,
      user: '',
      comments: [],
      page: 1,
      length: 0,
      displayComments: [],
      pageSize: 3
    }
  },
  mounted () {
    onAuthStateChanged(auth, (user) => {
      this.user = user
    })
    onSnapshot(usersCollectionRef, (querySnapshot) => {
      this.comments = querySnapshot.docs.map(doc => ({ ...doc.data(), id: doc.id }))
      this.comments.sort((a, b) => b.timestamp - a.timestamp)
      this.length = Math.ceil(this.comments.length / this.pageSize)
      this.displayComments = this.comments.slice(0, this.pageSize)
    })
  },
  methods: {
    remove (id) {
      const userDocumentRef = doc(db, 'blogs', id)
      deleteDoc(userDocumentRef)
    },
    commentRemove (id) {
      const userDocumentRef = doc(db, 'comments', id)
      deleteDoc(userDocumentRef)
    // },
    // closeUpdateForm () {
    //   this.isUpdate = false
    // },
    // closeCommentForm () {
    //   this.isComment = false
    },
    pageChange (pageNumber) {
      this.displayComments = this.comments.slice(this.pageSize * (pageNumber - 1), this.pageSize * (pageNumber))
    }
  }
}
</script>

<style>

</style>
