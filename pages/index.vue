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
        <Blog-List />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import { onAuthStateChanged, signOut } from '@firebase/auth'
import { auth } from '../plugins/firebase'
export default {
  name: 'IndexPage',
  data () {
    return {
      drawer: false,
      user: '',
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
          icon: '',
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
  },
  methods: {
    logout () {
      signOut(auth).then(() => (this.user = null))
    }
  }
}
</script>
