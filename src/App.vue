<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark>     
    </v-app-bar>

    <v-main>
      <LazyChat/>
    </v-main>
  </v-app>
</template>

<script>
// import HelloWorld from './components/HelloWorld';
import LazyChat from './components/Chat';

export default {
  name: 'App',

  components: {
    // HelloWorld,
    LazyChat
  },

  data: () => ({
    //
  }),
};
</script>
