<template>
  <v-app>
     <Main />
  </v-app>
</template>

<script>


import Main from './components/Main'

export default {
  name: 'App',
  components: {
    
    Main
    },
  data () {
    return {
      msg : 'hello'
    }
  }
}
</script>
