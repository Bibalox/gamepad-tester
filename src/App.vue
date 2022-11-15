<script setup>
import { reactive } from 'vue'

const gamepad = reactive({
  connected: false,
  buttons: []
})

window.addEventListener('gamepadconnected', e => {
  gamepad.connected = true
  gamepad.id = e.gamepad.id
  gamepad.mapping = e.gamepad.mapping

  loop()

  console.log(e.gamepad)
})

window.addEventListener('gamepaddisconnected', () => {
  gamepad.connected = false
})

function loop () {
  const request = requestAnimationFrame(loop)
  const frameState = navigator.getGamepads()[0]

  if (gamepad.connected) {
    gamepad.axes = frameState.axes.map(tata => tata)
    gamepad.buttons = frameState.buttons.map(tata => tata)
  } else {
    cancelAnimationFrame(request)
  }
}
</script>

<template>
  <template v-if="!gamepad.connected">
    <h1>No gamepad detected</h1>
    <p>When plugged in, press any button to connect it</p>
  </template>
  
  <template v-else>
    <h1>Gamepad connected</h1>
    <p>
      {{ 'id: ' + gamepad.id }}
      <br>
      {{ 'mapping: ' + gamepad.mapping }}
    </p>
    <div class="tables">
      <table>
        <thead>
          <tr>
            <th colspan="2">
              buttons
            </th>
          </tr>
          <tr>
            <th>id</th>
            <th>value</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="button, index in gamepad.buttons" :key="index">
            <td>{{ index }}</td>
            <td>{{ button.value }}</td>
          </tr>
        </tbody>
      </table>
      <table v-if="gamepad.axes.length !== 0">
        <thead>
          <tr>
            <th colspan="2">
              Axes
            </th>
          </tr>
          <tr>
            <th>id</th>
            <th>value</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="axe, index in gamepad.axes" :key="index">
            <td>{{ index }}</td>
            <td>{{ Math.round(axe , .2) }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
</template>

<style lang="scss">
  body {
    margin: 0;
    padding: 0;
  }

  #app {
    align-items: center;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    display: flex;
    flex-direction: column;
    justify-content: center;
    min-height: 100vh;
    text-align: center;
    width: 100%;
  }

  .tables {
    align-items: flex-start;
    display: flex;
    gap: 50px;
  }

  table {
    width: 150px;
  }
</style>
