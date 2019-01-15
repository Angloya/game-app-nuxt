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
        <v-btn  v-if="!isGameStarted" 
        @click="startGame">
          New game
        </v-btn>
      </div>
      <v-card max-height="500" max-width="500">
        <v-container v-bind="{ [`grid-list-${size}`]: true }" fluid>
          <v-layout row wrap>
            <v-flex
              v-for="(card, idx) in cards" :key="idx"
              xs3>
              <v-card flat 
              @keyup.left="moveToTheLeft"
              light
              :color="card.active ? card.colorActiveCard : colorCard"
              height="100"
              width="100">
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
      size: 'lg',
      isGameStarted: false,
      colorCard: 'grey darken-2',
      MoveActive: false,
      StringsLeft: {
        FirstStringX: [3, 2, 1, 0],
        SecondStringX: [7, 6, 5, 4],
        TreeStringX: [11, 10, 9, 8],
        FourdStringX: [15, 14, 13, 12],
      },
      StringsRight: {
        FirstStringX: [0, 1, 2, 3],
        SecondStringX: [4, 5, 6, 7],
        TreeStringX: [8, 9, 10, 11],
        FourdStringX: [12, 13, 14, 15],
      },
      StringsDown: {
        FirstStringX: [0, 4, 8, 12],
        SecondStringX: [1, 5, 9, 13],
        TreeStringX: [2, 6, 10, 14],
        FourdStringX: [3, 7, 11, 15],
      },
      StringsUp: {
        FirstStringX: [12, 8, 4, 0],
        SecondStringX: [13, 9, 5, 1],
        TreeStringX: [14, 10, 6, 2],
        FourdStringX: [15, 11, 7, 3],
      },
      cards: [
        {
          id: 0,
          active: false,
          num: null,
          colorActiveCard: 'teal'
        },
        {
          id: 1,
          active: false,
          num: null,
          colorActiveCard: 'teal'
        },
        {
          id: 2,
          active: false,
          num: null,
          colorActiveCard: 'teal'
        },
        {
          id: 3,
          active: false,
          num: null,
          colorActiveCard: 'teal'
        },
        {
          id: 4,
          active: false,
          num: null,
          colorActiveCard: 'teal'
        },
        {
          id: 5,
          active: false,
          num: null,
          colorActiveCard: 'teal'
        },
        {
          id: 6,
          active: false,
          num: null,
          colorActiveCard: 'teal'
        },
        {
          id: 7,
          active: false,
          num: null,
          colorActiveCard: 'teal'
        },
        {
          id: 8,
          active: false,
          num: null,
          colorActiveCard: 'teal'
        },
        {
          id: 9,
          active: false,
          num: null,
          colorActiveCard: 'teal'
        },
        {
          id: 10,
          active: false,
          num: null,
          colorActiveCard: 'teal'
        },
        {
          id: 11,
          active: false,
          num: null,
          colorActiveCard: 'teal'
        },
        {
          id: 12,
          active: false,
          num: null,
          colorActiveCard: 'teal'
        },
        {
          id: 13,
          active: false,
          num: null,
          colorActiveCard: 'teal'
        },
        {
          id: 14,
          active: false,
          num: null,
          colorActiveCard: 'teal'
        },
        {
          id: 15,
          active: false,
          num: null,
          colorActiveCard: 'teal'
        }
      ]
    }
  },
  methods: {
    startGame () {
      this.isGameStarted = true
      this.getNewActiveCard()
      this.getNewActiveCard()
      window.addEventListener('keyup', this.logKey)
      this.cardsClone = this.cards
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
          this.moveX(this.StringsLeft)
          break;
        case 39:
          this.moveX(this.StringsRight)
          break;
        case 40:
          this.moveX(this.StringsDown)
          break;
        case 38:
          this.moveX(this.StringsUp)
      }
    },
    cellActive (nextEl, el, rate) {
      this.cards[nextEl].num = this.cards[el].num * rate
      if (this.cards[nextEl].num < 32) {
      this.cards[nextEl].colorActiveCard = 'teal  accent-' + (this.cards[nextEl].num / 4)
      } else {
        this.cards[nextEl].colorActiveCard = 'teal  darken-' + (this.cards[nextEl].num / 16)
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
              this.cellActive(cardNext, card, 2)
              }
            } else {
              this.cellActive(cardNext, card, 1)
            }
          }
        }
      }
      if (this.MoveActive) {
      let newActiveCard = this.getNewActiveCard()
      this.MoveActive = false
      }
    }
  }
}
</script>
