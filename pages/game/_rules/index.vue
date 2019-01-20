<template>
  <v-layout
    column
    justify-center
    align-center>
    <v-card flat max-width="700">
      <v-card-text>
        <v-layout align-center mb-3>
          <strong class="display-2">RULES</strong>
        </v-layout>
        <p class="subheading text-center">
          2048 is played on a gray 4×4 grid, 
          with numbered tiles that slide smoothly when a player moves them using the four arrow keys. Every turn, 
          a new tile will randomly appear in an empty spot on the board with a value of either 2 or 4.
        </p>
        <p> Tiles slide as far as possible in the chosen direction until they are stopped by either another tile or the edge of the grid.
           If two tiles of the same number collide while moving, they will merge into a tile with the total value of the two tiles that collided.
            The resulting tile cannot merge with another tile again in the same move. Higher-scoring tiles emit a soft glow.
        </p>
        <p>
          A scoreboard on the upper-right keeps track of the user's score. The user's score starts at zero,
           and is incremented whenever two tiles combine, by the value of the new tile.
          As with many arcade games, the user's best score is shown alongside the current score.
        </p>
      </v-card-text>
    </v-card>
  </v-layout>
</template>