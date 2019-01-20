<template>
  <v-layout
    column
    justify-center
    align-center>
    <v-flex
      xs12
      sm8
      md6>
      <div class="text-xs-center">
      </div>
      <v-card>
        <v-layout
    column
    justify-center
    align-center>
       <v-btn nuxt to="/game">
          Game 2048
        </v-btn>
        <v-card-text>
          <h1>
            Join the numbers and get to the 2048 tile
          </h1>
        </v-card-text>
        <v-img min-width="350" src="/img/2048game.png"
          alt="2048"/>
        </v-layout>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import Logo from '~/components/Logo.vue'

export default {
  components: {
    Logo
  }
}
</script>
