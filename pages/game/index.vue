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
              :color="card.active ? colorActiveCard : colorCard"
              height="100"
              width="100">
                {{card.num}}
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
      colorCard: 'cyan darken-2',
      colorActiveCard: 'red',
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
        FirstStringX: [15, 11, 7, 3],
        SecondStringX: [14, 6, 6, 2],
        TreeStringX: [13, 9, 5, 2],
        FourdStringX: [12, 8, 4, 0],
      },
      cards: [
        {
          id: 0,
          active: false,
          num: null
        },
        {
          id: 1,
          active: false,
          num: null
        },
        {
          id: 2,
          active: false,
          num: null
        },
        {
          id: 3,
          active: false,
          num: null
        },
        {
          id: 4,
          active: false,
          num: null
        },
        {
          id: 5,
          active: false,
          num: null
        },
        {
          id: 6,
          active: false,
          num: null
        },
        {
          id: 7,
          active: false,
          num: null
        },
        {
          id: 8,
          active: false,
          num: null
        },
        {
          id: 9,
          active: false,
          num: null
        },
        {
          id: 10,
          active: false,
          num: null
        },
        {
          id: 11,
          active: false,
          num: null
        },
        {
          id: 12,
          active: false,
          num: null
        },
        {
          id: 13,
          active: false,
          num: null
        },
        {
          id: 14,
          active: false,
          num: null
        },
        {
          id: 15,
          active: false,
          num: null
        }
      ]
    }
  },
  methods: {
    startGame () {
      this.isGameStarted = true
      let newActiveCard = this.getNewActiveCard()
      window.addEventListener('keyup', this.logKey)
    },
    getNewActiveCard () {
      let newActiveCard = Math.floor(Math.random() * ((this.cards.length + 1) - 1)) + 1
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
          this.moveToTheLeft()
          break;
        case 39:
          this.moveToTheRight()
          break;
        case 40:
          this.moveToTheDown()
          break;
        case 38:
          this.moveToTheUp()
      }
    },
    moveToTheLeft () {
      for (let item in this.StringsLeft) {
        var string = this.StringsLeft[item]
        string.forEach(el => {
          if (this.cards[el].num && this.cards[el].id !== string[3]) {
            if (this.cards[el-1].num) {
              if ( this.cards[el].num === this.cards[el-1].num) {
              this.cards[el-1].num = this.cards[el].num*2
              this.cards[el].num = null
              this.cards[el-1].active = true;
              this.cards[el].active = false;
              }
            } else {
              this.cards[el-1].num = this.cards[el].num
              this.cards[el-1].active = true;
              this.cards[el].num = null
              this.cards[el].active = false;
            }
          }
        })
      }
      let newActiveCard = this.getNewActiveCard()
    },
    moveToTheRight () {
      for (let item in this.StringsRight) {
        var string = this.StringsRight[item]
        string.forEach(el => {
          if (this.cards[el].num && this.cards[el].id !== string[3]) {
            if (this.cards[el+1].num) {
              if ( this.cards[el].num === this.cards[el+1].num) {
              this.cards[el+1].num = this.cards[el].num*2
              this.cards[el].num = null
              this.cards[el+1].active = true;
              this.cards[el].active = false;
              }
            } else {
              this.cards[el+1].num = this.cards[el].num
              this.cards[el+1].active = true;
              this.cards[el].num = null
              this.cards[el].active = false;
            }
          }
        })
      }
      let newActiveCard = this.getNewActiveCard()
      //когда двигамем вправо не всегда двигается
    },
    moveToTheDown () {
       for (let item in this.StringsDown) {
        var string = this.StringsDown[item]
        for (let el in string) {
          if (this.cards[string[el]].num && this.cards[[string[el]]].id !== string[3]) {
            if (this.cards[[string[+el+1]]].num) {
              if ( this.cards[[string[el]]].num === this.cards[[string[+el+1]]].num) {
              this.cards[[string[+el+1]]].num = this.cards[[string[el]]].num*2
              this.cards[[string[+el]]].num = null
              this.cards[[string[+el+1]]].active = true;
              this.cards[[string[+el]]].active = false;
              }
            } else {
              this.cards[[string[+el+1]]].num = this.cards[[string[el]]].num
              this.cards[[string[+el+1]]].active = true;
              this.cards[[string[el]]].num = null
              this.cards[[string[el]]].active = false;
            }
          }
        }
      }
      let newActiveCard = this.getNewActiveCard()
    },
    moveToTheUp () {
      for (let item in this.StringsUp) {
        var string = this.StringsUp[item]
        string.forEach(el => {
          if (this.cards[el].num && this.cards[el].id !== string[3]) {
            if (this.cards[el+1].num) {
              if ( this.cards[el].num === this.cards[el+1].num) {
              this.cards[el+1].num = this.cards[el].num*2
              this.cards[el].num = null
              this.cards[el+1].active = true;
              this.cards[el].active = false;
              }
            } else {
              this.cards[el+1].num = this.cards[el].num
              this.cards[el+1].active = true;
              this.cards[el].num = null
              this.cards[el].active = false;
            }
          }
        })
      }
      let newActiveCard = this.getNewActiveCard()
    }

  }
}
</script>
