<script setup>
import { ref } from 'vue'

let buttonStyle = ref({ top: '', right: '', bottom: '', left: '' })
let cashOutButtonStatus = ref(false)

function getRandomInt (min, max) {
  min = Math.ceil(min)
  max = Math.floor(max)
  return Math.floor(Math.random() * (max - min + 1)) + min
}

function mouseOverCashOutButton () {
  const chance = Math.random()
  if (chance < 0.5) {
    moveButton()
    console.log('if1')
  }
  if (chance >= 0.5 && chance < 0.9)
  {
    unClickableButton()
    console.log('if2')
  }
}

function moveButton () {
  const randomDirectionNumber = getRandomInt(0, 3)
  switch (randomDirectionNumber) {
    case 1:
      buttonStyle.value.top = '300px'
      buttonStyle.value.right = ''
      buttonStyle.value.bottom = ''
      buttonStyle.value.left = ''
      break
    case 2:
      buttonStyle.value.top = ''
      buttonStyle.value.right = '300px'
      buttonStyle.value.bottom = ''
      buttonStyle.value.left = ''
      break
    case 3:
      buttonStyle.value.top = ''
      buttonStyle.value.right = ''
      buttonStyle.value.bottom = '300px'
      buttonStyle.value.left = ''
      break
    case 4:
      buttonStyle.value.top = ''
      buttonStyle.value.right = ''
      buttonStyle.value.bottom = ''
      buttonStyle.value.left = '300px'
      break
    default:
  }
}

function unClickableButton () {
  buttonStyle.value['pointer-events'] = 'none'
  buttonStyle.value['cursor'] = 'not-allowed'
  buttonStyle.value.opacity = '0.5'
}
</script>
<template>
  <button class="cash-out-button" @mouseover="mouseOverCashOutButton" type="button" :style="buttonStyle"
          :disabled="cashOutButtonStatus">CASH OUT
  </button>
</template>

<style scoped>
.cash-out-button {
  position: absolute;
  height: 45px;
  width: 140px;
}
</style>
