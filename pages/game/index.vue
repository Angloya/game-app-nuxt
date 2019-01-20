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
        <div v-if="!isGameStarted" >
          <v-btn @click="startGame">
            New game
          </v-btn>
          <v-btn nuxt to="/game/rules">
            Game rules
          </v-btn>
          <h1>{{message}}</h1>
        </div>
        <div v-if="isGameStarted" >
          <v-btn @click="overGame">
            Finish the game
          </v-btn>
          <h1>{{message}}</h1>
          <h1>Score: {{score}}</h1>
        </div>
      </div>
      <v-card max-height="500" max-width="500">
        <v-container grid-list-sm fluid>
          <v-img src="/img/2048game.png"
          alt="2048"
          max-height="500" max-width="500"
          v-if="!isGameStarted"/>
          <Cards
          :_isGameStarted="isGameStarted"
          :_cards="cards"
          :_strings="strings"
          :_fieldSize="size"
          @score="scoreCount($event)"
          />
        </v-container>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import Cards from '~/components/Cards.vue'
export default {
  name: "game",
  components: {
    Cards
  },
  data () {
    return {
      isGameStarted: false,
      size: 16,
      score: 0,
      message: 'Join the numbers and get to the 2048 tile',
      strings: {
        StringsLeft: {},
        StringsRight: {},
        StringsDown: {},
        StringsUp: {}
      },
      cards: []
    }
  },
  methods: {
    startGame () {
      this.isGameStarted = true
      this.getCards()
      this.getStringsX()
      this.getStringsY()
    },
    scoreCount (e) {
      this.score = e
    },
    overGame () {
      this.isGameStarted = false
      this.score = 0
      this.cards = []
      this.strings= {
        StringsLeft: {},
        StringsRight: {},
        StringsDown: {},
        StringsUp: {}
      }
    },
    getStringsX () {
      var strings = {}
      for (let i = 0; i < Math.sqrt(this.size); i++) {
        let string = []
        var sizeSqrt = Math.sqrt(this.size)*i
        var num = Math.sqrt(this.size)*(i+1) 
        for (let item = sizeSqrt; item < num; item++) {
          string.push(item)
        }
        strings[i] = string
      }
      this.strings.StringsRight = strings
      this.strings.StringsLeft = JSON.parse(JSON.stringify(this.strings.StringsRight))
      for (var string in this.strings.StringsLeft) {
        this.strings.StringsLeft[string].reverse()
      }
    },
    getStringsY () {
      var strings = {}
      for (let i = 0; i < Math.sqrt(this.size); i++) {
        let string = []
        var num = Math.sqrt(this.size) 
        for (let item = i; item < this.size; item+=num) {
          string.push(item)
        }
        strings[i] = string
      }
      this.strings.StringsDown = strings
      this.strings.StringsUp = JSON.parse(JSON.stringify(this.strings.StringsDown))
      for (var string in this.strings.StringsUp) {
        this.strings.StringsUp[string].reverse()
      }
    },
    getCards () {
    for (let item = 0; item < this.size; item++) {
        let card = {}
        card.id = item
        card.active = false,
        card.num = null,
        card.colorActiveCard = 'teal'
        this.cards.push(card)
      }
    }
  }
}
</script>
