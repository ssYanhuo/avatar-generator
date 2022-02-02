<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">

        <div class="mt-1">
          生成你的虎年专属头像
        </div>
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://www.neuq.edu.cn/"
        target="_blank"
        text
      >
        <span class="mr-2">学校官网</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <Generator/>
    </v-main>
  </v-app>
</template>

<script>
import Generator from './components/Generator';

export default {
  name: 'App',

  components: {
    Generator: Generator,
  },

  data: () => ({
    //
  }),
};
</script>
