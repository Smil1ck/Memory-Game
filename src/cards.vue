<script setup>
import {ref} from 'vue';

defineProps({
  item: Object,
  disabled: Boolean
});
</script>

<template>
  <div :class="{cardsoff: item.Flipped === false, cardson: item.Flipped === true, disabled: disabled}" 
  :style = "{backgroundColor: item.Flipped === true ? `rgb(${item.r},${item.g},${item.b})` : `lightskyblue`}"
  >
    <img class="card-img" v-if="item.Flipped === true" :src="item.image"> </img>
  </div>
</template>

<style scoped>
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
  align-items: center;
}

.disabled{
  pointer-events: none;
}

.card-img{
  width: 120px;
  height: 120px;
}

</style>