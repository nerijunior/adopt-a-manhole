<template>
  <div id="app">
    <loader v-if="loading"></loader>
    <router-view></router-view>
  </div>
</template>

<script>
import Loader from '@/components/Loader'

export default {
  name: 'app',
  components: {
    Loader
  },
  computed: {
    loading () {
      return this.$store.state.loading
    }
  },
  methods: {
    getUserProfile () {
      this.$store.commit('LOADING', true)

      window.axios.get('/me')
        .then(response => {
          this.$store.commit('SET_USER', response.data)
        })
        .catch(error => {
          console.error(error)
        })
        .then(() => {
          this.$store.commit('LOADING', false)
        })
    }
  },
  mounted () {
    const token = window.localStorage.getItem('token')

    if (token) {
      window.axios.defaults.headers.common['Authorization'] = 'Bearer ' + token
      this.getUserProfile()
    }
  }
}
</script>

<style lang="scss">
@import './assets/app.scss';
</style>
