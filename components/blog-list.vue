<template>
  <v-container>
    <v-app>
      <v-content>
        <div class="text-center">
          <v-list v-for="blog in displayBlogs" :key="blog.index">
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
          </v-list>
          <v-pagination v-model="page" :length="length" @input="pageChange" />
        </div>
      </v-content>
    </v-app>
  </v-container>
</template>

<script>
import { collection, deleteDoc, doc, onSnapshot } from '@firebase/firestore'
import { db } from '../plugins/firebase'
const usersCollectionRef = collection(db, 'blogs')

export default {
  name: 'BlogList',
  data () {
    return {
      title: '',
      content: '',
      page: 1,
      length: 0,
      blogs: [],
      displayBlogs: [],
      pageSize: 3,
      isUpdate: false
    }
  },
  mounted () {
    onSnapshot(usersCollectionRef, (querySnapshot) => {
      this.blogs = querySnapshot.docs.map(doc => ({ ...doc.data(), id: doc.id }))
      this.length = Math.ceil(this.blogs.length / this.pageSize)
      this.displayBlogs = this.blogs.slice(0, this.pageSize)
    })
  },
  methods: {
    // pageChange: (pageNumber) => {
    //   this.displayBlogs = this.blogs.slice(this.pageSize * (pageNumber - 1), this.pageSize * (pageNumber))
    // }
    pageChange (pageNumber) {
      this.displayBlogs = this.blogs.slice(this.pageSize * (pageNumber - 1), this.pageSize * (pageNumber))
    },
    remove (id) {
      const userDocumentRef = doc(db, 'blogs', id)
      deleteDoc(userDocumentRef)
    },
    update () {

    }
  }
}
</script>

<style>

</style>
