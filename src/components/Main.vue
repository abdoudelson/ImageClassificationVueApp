<template>
    <v-content>

        <Toolbar />

        <div
            style="max-height: 300px;"
        >
        <v-container style="height: 800px;">

             
          <Second />
      
      </v-container>
        </div>
    </v-content>
</template>

<script>
import Toolbar from './Toolbar'
import Second from './Second'

export default {
  name: 'Main',
  components: {
    Toolbar,
    Second
    },

  props: {
    msg: String
  }
}
</script>