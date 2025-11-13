<script setup>
import { ref, computed, watch } from "vue";
import Cards from "./cards.vue";
import { cardsCon } from "./CardsContainer";

function shuffle() {
  cardList.value = cardList.value.sort(() => Math.random() - 0.5);
}

function check() {
  if (openCards.value[0].name === openCards.value[1].name) {
    matches.value += 1;
    openCards.value = [];
    Block.value = false;
  } else {
    openCards.value[0].Flipped = false;
    openCards.value[1].Flipped = false;
    openCards.value = [];
    Block.value = false;
  }
}

const cardList = ref([...cardsCon, ...cardsCon.map((item) => ({ ...item }))]);
const openCards = ref([]);
const Block = ref(false);
const AllMatches = cardList.value.length / 2;
const moves = ref(0);
const matches = ref(0);

const isTwoOpened = computed(() => {
  return openCards.value.length === 2;
});

watch(matches, () => {
  if (matches.value === AllMatches) {
    return alert("you win");
  }
});

watch(isTwoOpened, () => {
  if (!isTwoOpened.value) return;
  Block.value = true;
  moves.value += 1;
  setTimeout(check, 700);
});

function restartGame() {
  //cardList.value = [...cardsCon, ...cardsCon.map(item => ({ ...item }))];
  cardList.value.map((item) => {
    {
      item.Flipped = false;
    }
  });
  shuffle();
  openCards.value = [];
  Block.value = false;
  moves.value = 0;
  matches.value = 0;
}

function cardclick(index) {
  if (cardList.value[index].Flipped === true) return;
  openCards.value.push(cardList.value[index]);
  cardList.value[index].Flipped = true;
  console.log(isTwoOpened.value);
}

restartGame();
</script>

<template>
  <div class="game-wrapper">
    <div class="buttos-wrapper">
      <button class="game-buttons" @click="restartGame">New game</button>
    </div>

    <div class="main-form">
      <div class="form-description">
        <div class="form-text">🔄Moves: {{ moves }}</div>
        <div class="form-text">✅Matches: {{ matches }}/{{ AllMatches }}</div>
      </div>

      <div class="card-form">
        <Cards
          v-for="(item, index) in cardList"
          :key="index"
          :item
          :disabled="Block"
          @click="cardclick(index)"
        >
        </Cards>
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
  margin-left: 200px;
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
</style>
