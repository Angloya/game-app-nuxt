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
          <v-layout row wrap
          v-if="isGameStarted">
            <v-flex
              v-for="(card, idx) in cards" :key="idx"
              xs3>
              <v-card flat 
              @keyup.left="moveToTheLeft"
              lights
              :color="card.active ? card.colorActiveCard : colorCard"
              height="100">
              <v-layout fill-height align-center justify-center>
              <v-flex class="text-xs-center">
                <h1>{{card.num}}</h1>
                </v-flex>
                </v-layout>
              </v-card>
            </v-flex>
          </v-layout>
        </v-container>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>

export default {
  data () {
    return {
      isGameStarted: false,
      size: 16,
      score: 0,
      colorCard: 'grey darken-2',
      MoveActive: false,
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
      this.getNewActiveCard()
      this.getNewActiveCard()
      window.addEventListener('keyup', this.logKey)
    },
    overGame () {
      this.score =  0;
      this.isGameStarted = false
      this.cards = []
      this.MoveActive = false
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
    },
    gameOver (message) {
      this.isGameStarted = false
      this.message = message
    },
    getNewActiveCard () {
      let newActiveCard = Math.floor(Math.random() * ((this.cards.length + 1) - 1))
        if (this.cards[newActiveCard].active !== true) {
          this.cards[newActiveCard].active = true;
          this.cards[newActiveCard].num = this.randomNum()
      } else this.getNewActiveCard()
    },
    randomNum () {
      var randomNum = Math.floor(Math.random()*100)
      if (randomNum < 90) {
        return 2
      } 
      if (randomNum < 100) {
        return 2
      } 
    },
    logKey(e) {
      switch (e.keyCode) {
        case 37:
          this.moveX(this.strings.StringsLeft)
          break;
        case 39:
          this.moveX(this.strings.StringsRight)
          break;
        case 40:
          this.moveX(this.strings.StringsDown)
          break;
        case 38:
          this.moveX(this.strings.StringsUp)
      }
      if (this.MoveActive) {
        this.getNewActiveCard()
        this.MoveActive = false
      }
    },
    cellActive (nextEl, el, rate) {
      this.cards[nextEl].num = this.cards[el].num * rate
      if (this.cards[nextEl].num < 32) {
      this.cards[nextEl].colorActiveCard = 'teal  darken-' + (this.cards[nextEl].num / 4)
      } else if (this.cards[nextEl].num < 513) {
        this.cards[nextEl].colorActiveCard = 'cyan darken-' + (this.cards[nextEl].num / 32)
      } else {
        this.cards[nextEl].colorActiveCard = 'indigo darken-' + (this.cards[nextEl].num / 512)
      }
      this.cards[nextEl].active = true
      this.cards[el].num = null
      this.cards[el].colorActiveCard = 'teal'
      this.cards[el].active = false
      this.MoveActive = true
    },
    moveX (strings) {
      for (let item in strings) {
        var string = strings[item]
        for (let el in string) {
          let card = string[el]
          let cardNext = string[+el+1]
          if (this.cards[card].num && this.cards[card].id !== string[3]) {
            if (this.cards[cardNext].num) {
              if ( this.cards[card].num === this.cards[cardNext].num) {
              this.score += this.cards[card].num*2
              this.cellActive(cardNext, card, 2)
              } 
            } else {
              this.cellActive(cardNext, card, 1)
              this.moveX(strings)
            }
          }
        }
      }
    }
  }
}
</script>
