<template>
  <v-app>
    <v-app-bar
      app
      color="#42b983"
      dark
    >
    <div class="d-flex align-center">
        <v-img
          alt="HCV Logo"
          class="shrink mr-2"
          contain
          src="./assets/logo.png"
          transition="scale-transition"
          width="40"
        />
          <span class="pageName" color="#42b983">CENTRAL EXPRESS</span>
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="/home"
        target="_blank"
        text
      >
        <span class="mr-2">Inicio</span>
        <v-icon>mdi-home</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <SubidaArchivo/>
    </v-main>
  </v-app>
</template>

<script>
import SubidaArchivo from './components/SubidaArchivo.vue';

export default {
  name: 'App',

  components: {
    SubidaArchivo
},

  data: () => ({
    //
  }),
};
</script>

<style>
  .pageName{
    font-size: larger;
    font-family: Roboto;
  }
</style>