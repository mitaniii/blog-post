<template>
  <v-app>
    <v-navigation-drawer v-model="drawer" app>
      <v-list>
        <v-list-item v-for="item in items" :key="item.title" :to="item.to">
          <v-list-item-action>
            <v-icon>
              {{ item.icon }}
            </v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>
              {{ item.title }}
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar app>
      <v-toolbar-title class="mx-auto">
        <h1>
          Blog-Post-App
        </h1>
      </v-toolbar-title>
      <v-toolbar-subtitle v-if="user">
        <p>
          {{ user.displayName }}さん
          <v-btn @click="logout">
            ログアウト
          </v-btn>
        </p>
      </v-toolbar-subtitle>
      <v-toolbar-subtitle v-else>
        <nuxt-link to="/login">
          ログイン
        </nuxt-link>
      </v-toolbar-subtitle>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
    </v-app-bar>
    <v-main>
      <v-container>
        <v-form @submit.prevent="addBlog">
          <div v-if="user">
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
