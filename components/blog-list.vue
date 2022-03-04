<template>
  <v-container>
    <v-list v-for="blog in blogs" :key="blog.title">
      <v-card>
        <v-subheader>{{ blog.title }}</v-subheader>
        <v-list-item>{{ blog.content }}</v-list-item>
      </v-card>
    </v-list>
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
      blogs: [],
      title: '',
      content: ''
    }
  },
  // props: {
  //   blogs: { type: Array, default: null }
  // },
  mounted () {
    onSnapshot(usersCollectionRef, (querySnapshot) => {
      this.blogs = querySnapshot.docs.map(doc => ({ ...doc.data(), id: doc.id }))
    })
  }
}
</script>

<style>

</style>
