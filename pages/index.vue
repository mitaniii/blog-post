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
    <v-app-bar app color="#00E676">
      <v-app-bar-nav-icon @click="drawer = !drawer" />
      <v-toolbar-title class="mx-auto">
        <v-btn color="#00E676" to="/">
          ブログアプリ
        </v-btn>
      </v-toolbar-title>
      <v-spacer />
      <v-toolbar-subtitle v-if="user">
        {{ user.displayName }}さん
        <v-btn class="mx-5" @click="logout">
          ログアウト
        </v-btn>
      </v-toolbar-subtitle>
      <v-toolbar-subtitle v-else>
        <nuxt-link to="/login">
          ログイン
        </nuxt-link>
      </v-toolbar-subtitle>
    </v-app-bar>
    <v-container>
      <v-app>
        <v-content>
          <div class="text-center">
            <BlogCards v-for="blog in displayBlogs" :key="blog.index" :blog="blog" />
            <v-pagination v-model="page" :length="length" @input="pageChange" />
          </div>
        </v-content>
      </v-app>
    </v-container>
  </v-app>
</template>

<script>
import { collection, deleteDoc, doc, onSnapshot } from '@firebase/firestore'
import { onAuthStateChanged, signOut } from '@firebase/auth'
import { auth, db } from '../plugins/firebase'
import BlogCards from '../components/blog-cards'
const usersCollectionRef = collection(db, 'blogs')

export default {
  name: 'IndexPage',
  components: {
    BlogCards
  },
  data () {
    return {
      drawer: false,
      user: '',
      title: '',
      content: '',
      page: 1,
      length: 0,
      blogs: [],
      displayBlogs: [],
      pageSize: 3,
      items: [
        {
          icon: 'mdi-account-circle',
          title: 'login',
          to: '/login'
        },
        {
          icon: 'mdi-account-circle',
          title: 'signup',
          to: '/signup'
        },
        {
          icon: 'mdi-message-text',
          title: 'blogform',
          to: '/blog-form'
        }
      ]
    }
  },
  mounted () {
    onAuthStateChanged(auth, (user) => {
      this.user = user
    })
    onSnapshot(usersCollectionRef, (querySnapshot) => {
      this.blogs = querySnapshot.docs.map(doc => ({ ...doc.data(), id: doc.id }))
      this.blogs.sort((a, b) => b.timestamp - a.timestamp)
      this.length = Math.ceil(this.blogs.length / this.pageSize)
      this.displayBlogs = this.blogs.slice(0, this.pageSize)
    })
  },
  methods: {
    logout () {
      signOut(auth).then(() => (this.user = null))
    },
    pageChange (pageNumber) {
      this.displayBlogs = this.blogs.slice(this.pageSize * (pageNumber - 1), this.pageSize * (pageNumber))
    },
    remove (id) {
      const userDocumentRef = doc(db, 'blogs', id)
      deleteDoc(userDocumentRef)
    }
  }
}
</script>
