<template>
  <v-container>
    <v-form @submit.prevent="addBlog">
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
    </v-form>
  </v-container>
</template>

<script>
import { addDoc, collection } from '@firebase/firestore'
import { db } from '../plugins/firebase'
const usersCollectionRef = collection(db, 'blogs')

export default {
  name: 'BlogForm',
  data () {
    return {
      title: '',
      content: ''
    }
  },
  methods: {
    addBlog () {
      if (this.title.trim() && this.content.trim()) {
        addDoc(usersCollectionRef, {
          title: this.title,
          content: this.content
        }).then(() => {
          this.title = ''
          this.content = ''
        })
      }
    }
  }
}
</script>

<style>

</style>
