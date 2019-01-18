<template>
<v-layout row wrap v-if="_isGameStarted">
  <v-flex v-for="(card, idx) in _cards" :key="idx"
    xs3>
    <v-card flat 
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
</template>

<script>
export default {
  name: 'cards',
  props: {
    _isGameStarted: {
      type: Boolean,
      default: false
    },
    _fieldSize: {
      type: Number,
      default: 16
    },
    _cards: {
      type: Array
    },
    _strings: {
      type: Object
    }
  },
  data () {
    return {
      score: 0,
      colorCard: 'grey darken-2',
      MoveActive: false
    }
  },
  watch: {
    _isGameStarted () {
      if (this._isGameStarted) {
        this.startGame()
      } else {
        this.overGame()
      }
    }
  },
  methods: {
    startGame () {
      this.getNewActiveCard()
      this.getNewActiveCard()
      window.addEventListener('keyup', this.logKey)
    },
    overGame () {
      this.MoveActive = false
    },
    getNewActiveCard () {
      let newActiveCard = Math.floor(Math.random() * ((this._cards.length + 1) - 1))
        if (this._cards[newActiveCard].active !== true) {
          this._cards[newActiveCard].active = true;
          this._cards[newActiveCard].num = this.randomNum()
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
          this.moveX(this._strings.StringsLeft)
          break;
        case 39:
          this.moveX(this._strings.StringsRight)
          break;
        case 40:
          this.moveX(this._strings.StringsDown)
          break;
        case 38:
          this.moveX(this._strings.StringsUp)
      }
      if (this.MoveActive) {
        this.getNewActiveCard()
        this.MoveActive = false
      }
    },
    cellActive (nextEl, el, rate) {
      this._cards[nextEl].num = this._cards[el].num * rate
      if (this._cards[nextEl].num < 32) {
      this._cards[nextEl].colorActiveCard = 'teal  darken-' + (this._cards[nextEl].num / 4)
      } else if (this._cards[nextEl].num < 513) {
        this._cards[nextEl].colorActiveCard = 'cyan darken-' + (this._cards[nextEl].num / 32)
      } else {
        this._cards[nextEl].colorActiveCard = 'indigo darken-' + (this._cards[nextEl].num / 512)
      }
      this._cards[nextEl].active = true
      this._cards[el].num = null
      this._cards[el].colorActiveCard = 'teal'
      this._cards[el].active = false
      this.MoveActive = true
    },
    moveX (_strings) {
      for (let item in _strings) {
        var string = _strings[item]
        for (let el in string) {
          let card = string[el]
          let cardNext = string[+el+1]
          if (this._cards[card].num && this._cards[card].id !== string[3]) {
            if (this._cards[cardNext].num) {
              if ( this._cards[card].num === this._cards[cardNext].num) {
              this.score += this._cards[card].num*2
              this.$emit('score', this.score)
              this.cellActive(cardNext, card, 2)
              }
            } else {
              this.cellActive(cardNext, card, 1)
              this.moveX(_strings)
            }
          }
        }
      }
    }
  }
}
</script>
