<script setup>
import { ref, reactive } from 'vue'

const gamepads = ref(0)
const buttonPressed = reactive({
  amount: 0,
  last: 'None'
})

const gamepad = ref({})

window.addEventListener('gamepadconnected', e => {
  gamepads.value++
  gamepad.value = e.gamepad

  console.log(gamepad.value)
})

window.addEventListener('gamepaddisconnected', () => {
  gamepads.value--
})

function loop () {
  if (gamepad.value.buttons) {
    buttonPressed.amount = 0

    gamepad.value.buttons.forEach((button, index) => {
      if (button.pressed) {
        buttonPressed.amount++
        buttonPressed.last = [index + ' (value: ' + button.value + ')']
      }
    })

    if (buttonPressed.amount === 0) buttonPressed.last = 'None'
  }

  window.requestAnimationFrame(loop)
}

window.requestAnimationFrame(loop)
</script>

<template>
  <p>
    {{ `Gamepad(s) connected: ${ gamepads }` }}
  </p>
  <p>
    {{ `Button pressed: ${ buttonPressed.last }` }}
  </p>
</template>

<style lang="scss">
  body {
    margin: 0;
    padding: 0;
  }

  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    display: flex;
    flex-direction: column;
    justify-content: center;
    min-height: 100vh;
    width: 100%;
  }

  p {
    margin: 0;
    text-align: center;
  }
</style>
