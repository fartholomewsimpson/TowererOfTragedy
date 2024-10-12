<template>
  <div>
    <div v-if="!entered">
      <EntranceScreen @enter=enter />
    </div>
    <div v-else-if="won">
      <WinScreen @reset="reset" />
    </div>
    <div v-else>
      <TowerOfTragedy @win=win />
    </div>
  </div>
</template>

<script>
import EntranceScreen from './components/EnteranceScreen.vue'
import TowerOfTragedy from './components/TowerOfTragedy.vue';
import WinScreen from './components/WinScreen.vue';

export default {
  name: 'App',
  components: {
    EntranceScreen,
    TowerOfTragedy,
    WinScreen,
  },
  data() {
    return {
      entered: false,
      won: false,
    };
  },
  methods: {
    enter: function() {
      this.entered = true;
    },
    win: function() {
      this.won = true;
    },
    reset: function() {
      this.won = false;
      this.entered = false;
    },
  },
}
</script>

<style>
body {
  margin: 0;
  background-size: 200px;
  background-image: url('./assets/background.png');
  background-repeat: repeat;
}
#app {
  font-family: Comic Sans, Comic Sans MS, Arial, sans-serif;
  font-weight: bold;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ffffffc0;
  min-height: 100vh;
  background-color: rgba(85, 43, 210, 0.5);
}
button {
  font-family: Comic Sans, Comic Sans MS, Arial, sans-serif;
  font-weight: bold;
  font-size: 40px;
  min-width: 100px;
  height: 100px;
  margin-left: 10px;
  margin-right: 10px;
  color: #ffffff;
  border-radius: 10px;
  border: 2px solid rgb(36, 36, 36);
  /* box-shadow: 4px 4px black; */
}
button:active {
  -webkit-box-shadow: inset 0px 0px 5px black;
     -moz-box-shadow: inset 0px 0px 5px black;
          box-shadow: inset 0px 0px 5px black;
  outline: none;
}
img {
  max-width: 90%;
}
</style>
