<template>
  <v-container>
    <div>登録が完了しました</div>
    <p>{{ user.displayName }}さん</p>
    <nuxt-link to="/">
      一覧ページへ
    </nuxt-link>
  </v-container>
</template>

<script>
import { onAuthStateChanged } from '@firebase/auth'
import { auth } from '../plugins/firebase'
export default {
  name: 'NoticePage',
  data () {
    return {
      user: ''
    }
  },
  mounted () {
    onAuthStateChanged(auth, (user) => {
      this.user = user
    })
  }

}
</script>

<style>

</style>
