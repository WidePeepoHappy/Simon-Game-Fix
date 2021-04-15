<template>
  <div class="game-container">
    <div id="main-circle">
      <div
        v-for="(circlePartColor, index) in circleColors"
        :key="circlePartColor"
        @[playerClick]="playerMoveColor(index)"
        :style="{ backgroundColor: circlePartColor }"
        :id="circlePartsPositions(index)"
      ></div>
    </div>
    <button @[playerCanReStart]="startGame" class="button" id="start">
      Старт
    </button>
    <h2>Раунд: {{ round }}</h2>
    <h2 v-if="lost">
      Вы проиграли после {{ round }} {{ round === 1 ? "раунда" : "раундов" }}
    </h2>
  </div>
</template>

<script>
export default {
  props: ["mode"],
  data() {
    return {
      circleColors: ["darkgreen", "darkred", "goldenrod", "darkblue"],
      color1: "darkgreen",
      color2: "darkred",
      color3: "goldenrod",
      color4: "darkblue",
      sequence: [],
      playerSequence: [],
      playerTurn: false,
      canReStart: true,
      lost: false,
      round: 0,
      intervalId: null,
    };
  },
  computed: {
    playerClick() {
      return this.playerTurn ? "click" : null;
    },
    playerCanReStart() {
      return this.canReStart ? "click" : null;
    },
  },
  methods: {
    circlePartsPositions(index) {
      if (index === 0) {
        return "topleft";
      } else if (index === 1) {
        return "topright";
      } else if (index === 2) {
        return "bottomleft";
      } else if (index === 3) {
        return "bottomright";
      }
    },
    startGame() {
      this.sequence = [];
      this.playerSequence = [];
      this.round = 1;
      this.lost = false;
      this.playerTurn = false;
      this.newRound();
    },
    newRound() {
      this.canReStart = false;
      this.sequence.push(Math.floor(Math.random() * 4) + 1);
      this.initSequence(this.sequence);
    },
    initSequence(sequence) {
      let i = 0;
      this.intervalId = setInterval(() => {
        this.clearColor();

        if (i >= sequence.length) {
          clearInterval(this.intervalId);
          this.playerTurn = true;
          this.canReStart = true;
        } else {
          setTimeout(() => {
            this.flashTileNumber(sequence[i]);
            i++;
          }, 200);
        }
      }, this.checkMode());
    },
    checkMode() {
      if (this.mode === "easy") {
        return 1500;
      } else if (this.mode === "medium") {
        return 1000;
      } else if (this.mode === "hard") {
        return 400;
      }
    },
    playerMoveColor(index) {
      if (index === 0) {
        this.playerMove(1);
      } else if (index === 1) {
        this.playerMove(2);
      } else if (index === 2) {
        this.playerMove(3);
      } else if (index === 3) {
        this.playerMove(4);
      }
    },
    playerMove(circlePart) {
      this.playerSequence.push(circlePart);
      this.check();
      this.flashTileNumber(circlePart);
      setTimeout(() => {
        this.clearColor();
      }, 300);
    },
    check() {
      if (
        this.playerSequence[this.playerSequence.length - 1] !==
        this.sequence[this.playerSequence.length - 1]
      ) {
        this.lost = true;
        this.playerTurn = false;
        this.clearColor();
      }
      if (this.round == this.playerSequence.length && !this.lost) {
        this.round++;
        this.playerSequence = [];
        this.playerTurn = false;
        this.newRound();
      }
    },
    flashTileNumber(tile) {
      if (tile === 1) {
        this.flashTile(0, "lightgreen", "clip1");
      } else if (tile === 2) {
        this.flashTile(1, "tomato", "clip2");
      } else if (tile === 3) {
        this.flashTile(2, "yellow", "clip3");
      } else if (tile === 4) {
        this.flashTile(3, "lightskyblue", "clip4");
      }
    },
    flashTile(position, color, clip) {
      this.circleColors[position] = color;
      let audio = document.getElementById(clip);
      audio.currentTime = 0;
      audio.play();
    },
    clearColor() {
      this.circleColors[0] = "darkgreen";
      this.circleColors[1] = "darkred";
      this.circleColors[2] = "goldenrod";
      this.circleColors[3] = "darkblue";
    },
  },
};
</script>

<style lang="scss">
@mixin size($width, $height) {
  width: $width;
  height: $height;
}

@mixin border-radius($radius, $color) {
  border-radius: $radius;
  -moz-border-radius: $radius;
  -webkit-border-radius: $radius;
  background: $color;
}

.game-container {
  display: flex;
  flex-direction: column;
}
#start {
  font-size: 25px;
  background-color: lightblue;
  margin-top: 20px;
}

#main-circle {
  @include size(300px, 300px);
  background: #385a94;
  border-radius: 50%;
  position: relative;
  border: solid 5px;
  margin-right: 20px;
}

#topleft,
#topright,
#bottomleft,
#bottomright {
  @include size(150px, 150px);
  position: absolute;
  top: 50%;
  left: 50%;
  border: solid 2.5px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}

#topleft {
  @include border-radius(150px 0 0 0, darkgreen);
  margin: -150px 0px 0px -150px;
}

#topright {
  @include border-radius(0 150px 0 0, darkred);
  margin: -150px 0px 0px 0px;
}

#bottomleft {
  @include border-radius(0 0 0 150px, goldenrod);
  margin: 0px -150px 0px -150px;
}

#bottomright {
  @include border-radius(0 0 150px 0, darkblue);
  margin: 0px 0px -150px 0px;
}
</style>