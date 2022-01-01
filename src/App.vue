<template>
  <v-app>
    <v-app-bar
        app
        color="primary"
        dark
    >
    </v-app-bar>

    <v-main>

      <Calculator/>
    </v-main>
  </v-app>
</template>

<script>
import Calculator from "@/components/Calculator";

export default {
  name: 'App',

  components: {
    Calculator
  },

  data: () => ({
  }),
};
</script>


