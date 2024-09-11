<template>
  <div id="app">
    <!-- <PlayerDashboard /> -->
   <XOGame />
  </div>
</template>

<script>
import XOGame from './components/XOGame.vue'
// import PlayerDashboard from './components/PlayerDashboard.vue'

export default {
  name: 'App',
  components: {
    // PlayerDashboard
    XOGame
  }
}
</script>