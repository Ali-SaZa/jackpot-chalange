<script setup>
import { computed, ref } from 'vue'
import SymbolComponent from './SymbolComponent.vue'
import CashOutButton from './CashOutButton.vue'

let credit = ref(10)
let onRolling = ref(false)
let symbolsLoading = ref([false, false, false])
const symbols = ref(['cherries.png', 'lemon.png', 'orange.png', 'watermelon.png'])
let rollResult = ref([0, 1, 2])

function getRandomInt (min, max) {
  min = Math.ceil(min)
  max = Math.floor(max)
  return Math.floor(Math.random() * (max - min + 1)) + min
}

function randomChance (cheatRate) {
  return Math.random() <= cheatRate
}

const allEqual = array => array.every(val => val === array[0])

async function roll (decreaseCredit) {
  if (credit.value > 0) {
    credit.value -= decreaseCredit
    for (let i = 0; i < 3; i++) {
      rollResult.value[i] = getRandomInt(0, 3)
    }
    loadingFunction()
  } else {
    window.alert('You don\'t have enough credit!!!')
  }
}

function loadingFunction () {
  onRolling.value = true
  symbolsLoading.value.fill(true)
  setTimeout(() => symbolsLoading.value[0] = false, 1000)
  setTimeout(() => symbolsLoading.value[1] = false, 2000)
  setTimeout(() => {
      symbolsLoading.value[2] = false
      if (allEqual(rollResult.value)) {
        calculateCredit(rollResult.value[0])
      }
      onRolling.value = false
    }, 3000
  )
}

function calculateCredit (prizeIndex) {
  if (credit.value >= 40 && credit.value < 60) {
    randomChance(0.3) ? roll(0) : win(prizeIndex)
  } else if (credit.value >= 60) {
    randomChance(0.6) ? roll(0) : win(prizeIndex)
  } else {
    win(prizeIndex)
  }
}

function win(prizeIndex){
  window.alert('You win'+ (prizeIndex + 1) * 10 +'credit!!!')
  credit.value += (prizeIndex + 1) * 10
}

const symbolsIcon = computed(function () {
  return [
    {
      image: symbols.value[rollResult.value[0]]
    },
    {
      image: symbols.value[rollResult.value[1]]
    },
    {
      image: symbols.value[rollResult.value[2]]
    }
  ]
})
</script>

<template>
  <div class="credit-value">
    Your Credit is: {{credit}}
  </div>
  <div class="jackpot-container">
    <div class="symbols-container">
      <div class="symbol-items" v-for="(item,index) in symbolsIcon" :key="index">
        <SymbolComponent :image-url="item.image" :index="index" :loading="symbolsLoading[index]" />
      </div>
    </div>
    <div class="button-container">
      <button type="button" @click="roll(1)" :disabled="onRolling|| !credit">Start Game</button>
      <CashOutButton />
    </div>
  </div>
</template>

<style scoped>
.credit-value{
  text-align: left;
  font-size: 25px;
  color: azure;
}
.jackpot-container {
  width: 90vw;
  height: 90vh;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: row;
}
.button-container{
  margin-left: 20px;
  position: relative;
}


.symbols-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 450px;
}

.symbol-items {
  width: 125px;
  height: 125px;
  background-color: lightgrey;
  padding: 5px;
  border-radius: 4px;
}
</style>
