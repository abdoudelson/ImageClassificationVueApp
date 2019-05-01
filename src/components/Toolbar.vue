<template>
  <v-toolbar
    absolute
    color="teal lighten-2"
    dark
    scroll-off-screen
    scroll-target="#scrolling-techniques"
    class="mb-5"
  >
    <v-toolbar-side-icon></v-toolbar-side-icon>

    <v-toolbar-title>BreakHis Scanner</v-toolbar-title>

    <v-spacer></v-spacer>

    <v-btn icon>
      <v-icon>more_vert</v-icon>
    </v-btn>
  </v-toolbar>
</template>

<script>
export default {
  name: "Toolbar",

  props: {
    msg: String
  }
};
</script>