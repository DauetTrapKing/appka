<template>
  <v-app>
    <Appbar/>
    <v-main app>
      <Widget/>
    </v-main>
    <v-footer 
      padless
      app
    >
      <v-col class="text-center">
        All rights reserved.
      </v-col>
    </v-footer>
  </v-app>
</template>
<script>
import Widget from './components/widget.vue'
import Appbar from './components/appbar.vue'

export default {
  name: 'app',
  components:{
    Widget,
    Appbar,
  }
}
</script>
