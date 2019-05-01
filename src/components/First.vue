<template>


      <v-container fluid fill-height>
        <v-layout align-center justify-center>

    <v-jumbotron
     align-center justify-center
    class="elevation-10 "
    gradient="to top right, rgba(63,81,181, .7), rgba(25,32,72, .7)"
     dark
    src="https://cdn.vuetifyjs.com/images/parallax/material2.jpg"
    >
    <v-container fill-height>
      <v-layout align-center>
        <v-flex>
          <h3 class="display-3">Welcome to BreakHis Scanner</h3>

          <span class="subheading">Lorem ipsum dolor sit amet, pri veniam forensibus id. Vis maluisset molestiae id, ad semper lobortis cum. At impetus detraxit incorrupte usu, repudiare assueverit ex eum, ne nam essent vocent admodum.</span>

          <v-divider class="my-3"></v-divider>

          <div class="title mb-3">Click here to import an image !</div>

          <v-btn
            class="mx-0"
            color="pink"
            large
            dark
          >
            Import image
          </v-btn>
        </v-flex>
      </v-layout>
    </v-container>
  </v-jumbotron>
        </v-layout>
      </v-container>
</template>
<script>
export default {
    name: 'First'
}
</script>
