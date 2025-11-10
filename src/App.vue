<script setup>
import { ref, computed, watch } from "vue";
const cardList = ref([
  {
    r: 255,
    g: 0,
    b: 0,
    Flipped: false,
    notFlipped: true,
  },

  {
    r: 0,
    g: 255,
    b: 0,
    Flipped: false,
    notFlipped: true,
  },

  {
    r: 0,
    g: 0,
    b: 255,
    Flipped: false,
    notFlipped: true,
  },

  {
    r: 255,
    g: 255,
    b: 0,
    Flipped: false,
    notFlipped: true,
  },

  {
    r: 0,
    g: 255,
    b: 255,
    Flipped: false,
    notFlipped: true,
  },

  {
    r: 255,
    g: 0,
    b: 255,
    Flipped: false,
    notFlipped: true,
  },

  {
    r: 255,
    g: 0,
    b: 0,
    Flipped: false,
    notFlipped: true,
  },

  {
    r: 0,
    g: 255,
    b: 0,
    Flipped: false,
    notFlipped: true,
  },

  {
    r: 0,
    g: 0,
    b: 255,
    Flipped: false,
    notFlipped: true,
  },

  {
    r: 255,
    g: 255,
    b: 0,
    Flipped: false,
    notFlipped: true,
  },

  {
    r: 0,
    g: 255,
    b: 255,
    Flipped: false,
    notFlipped: true,
  },

  {
    r: 255,
    g: 0,
    b: 255,
    Flipped: false,
    notFlipped: true,
  },
]);
const flippedCards = ref([]);

const matches = ref(0);
const tries = ref(0);
const max_cards = 12;

const Block = ref(false);

function shuffle() {
  cardList.value = cardList.value.sort(() => Math.random() - 0.5);
}

watch(matches, () => {
  if (matches.value === max_cards / 2) {
    alert("You win");
    restart();
  }
});

function restart() {
  flippedCards.value = [];
  tries.value = 0;
  matches.value = 0;
  Block.value = false;
  currentUseCards.value = 0;
  cardList.value.map((item) => {
    item.Flipped = false;
    item.notFlipped = true;
  });
  shuffle();
}

function open(item) {
  item.Flipped = true;
  item.notFlipped = false;
}

function close(item) {
  item.Flipped = false;
  item.notFlipped = true;
}

function check() {
  if (
    flippedCards.value[0].r === flippedCards.value[1].r &&
    flippedCards.value[0].g === flippedCards.value[1].g &&
    flippedCards.value[0].b === flippedCards.value[1].b
  ) {
    matches.value += 1;
    flippedCards.value = [];
    Block.value = false;
  } else {
    close(flippedCards.value[0]);
    close(flippedCards.value[1]);
    flippedCards.value = [];
    Block.value = false;
  }
}

const currentUseCards = ref(0);

function cardclick(item) {
  if (currentUseCards.value === 2) return;
  if (item.Flipped) return;
  if (Block.value === true) return;

  currentUseCards.value += 1;
  flippedCards.value.push(item);
  open(item);

  if (currentUseCards.value === 2) {
    tries.value += 1;
    Block.value = true;
    setTimeout(check, 750);
    currentUseCards.value = 0;
  }
}
</script>

<template>
  <div class="game-wrapper">
    <div class="buttos-wrapper">
      <button class="game-buttons" @click="restart">Restart</button>
      <button class="game-buttons" @click="shuffle">Shuffle</button>
    </div>

    <div class="main-form">
      <div class="form-description">
        <div class="form-text">Tries: {{ tries }}</div>
        <div class="form-text">Matches: {{ matches }}</div>
      </div>

      <div class="card-form">
        <div
          v-for="(item, key) in cardList"
          @click="cardclick(item)"
          :class="{ cardson: item.Flipped, cardsoff: item.notFlipped }"
          :style="{
            backgroundColor: item.Flipped
              ? `rgb(${item.r},${item.g},${item.b})`
              : `lightskyblue`,
          }"
        >
          ?
        </div>
      </div>
    </div>
  </div>
</template>
<style scoped>
.game-wrapper {
  width: 700px;
  margin: 50px;
  height: 700px;
  display: inline-flex;
  flex-direction: column;
  outline: 5px solid black;
}
.buttos-wrapper {
  display: inline-flex;
  width: 700px;
  height: 200px;
  border-bottom: solid 5px black;
  background-color: #222222;
}

.game-buttons {
  width: 300px;
  margin-left: 25px;
  background-color: gray;
  font-size: 40px;
  font-weight: bold;
}

.main-form {
  width: 700px;
  height: 500px;
  display: inline-flex;
  flex-direction: column;
}

.form-description {
  width: 700px;
  height: 50px;
  border-bottom: 5px solid black;
  display: inline-flex;
  background-color: gray;
}

.form-text {
  margin: auto;
  font-size: 20px;
  font-weight: bold;
}

.card-form {
  width: 700px;
  height: 450px;
  display: inline-flex;
  flex-direction: column;
  flex-wrap: wrap;
}

.cardsoff {
  width: 150px;
  height: 120px;
  border: 3px solid black;
  margin: auto;
  background-color: lightskyblue;
  text-align: center;
  font-size: 80px;
  font-weight: bold;
}

.cardson {
  width: 150px;
  height: 120px;
  border: 3px solid black;
  margin: auto;
  text-align: center;
  font-size: 0px;
  font-weight: bold;
}
</style>
