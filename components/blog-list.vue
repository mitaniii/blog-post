<template>
  <v-container>
    <v-app>
      <v-content>
        <div class="text-center">
          <v-list v-for="blog in displayBlogs" :key="blog.index">
            <v-card>
              <v-subheader>{{ blog.title }}</v-subheader>
              <v-list-item>{{ blog.content }}</v-list-item>
            </v-card>
          </v-list>
          <v-pagination v-model="page" :length="length" @input="pageChange" />
        </div>
      </v-content>
    </v-app>
  </v-container>
</template>

<script>
import { collection, onSnapshot } from '@firebase/firestore'
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
      pageSize: 3
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
    }
  }
}
</script>

<style>

</style>
