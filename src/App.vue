<template>
  <v-app>
    <v-content>
     <v-container
        class=""
        fluid >
        <v-row
          align="start"
          justify="center" >
          <v-col
            cols="12"
            sm="8"
            md="6" >

              <Todos />

          </v-col>
        </v-row>
     </v-container>
    </v-content>
  </v-app>
</template>

<script>
import Todos from '@/components/Todos.vue';

export default {
  name: 'App',

  components: {
    Todos,
  },

};
</script>
