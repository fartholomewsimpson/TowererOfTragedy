<template>
  <div>
    <div v-if="showMap">
      <img src="../assets/isleOHags.png" />
    </div>
    <div v-else-if="showCharacters">
      <img src="../assets/characters.png" />
    </div>
    <div v-else-if="playSong">
      <img src="../assets/jukebox.png" />
    </div>
    <div v-else>
      <div class="title">
        <img alt="congratulations" src="../assets/theEnd.png"/>
      </div>
      <div class="content">
        <div class="textbox">
          <p>THAT'S THE END OF THE GAME. WHAT'S WRONG WITH THE SCORING SYSTEM? IT MUST BE BROKEN...</p>
        </div>
        <div class="textbox">
          <p>YOU CAN'T HAVE GOT THAT MANY POINTS! NO, NO, NO, NO, NO!</p>
        </div>
        <div class="textbox">
          <p>I DECLARE THE WIN INVALID, DUE TO FAULTY PROGRAMMING... IN LIEU OF A PRIZE, HERE'S THIS JUNK I USED FOR RESEARCH.</p>
        </div>
        <div class="buttons">
          <button @click="showMap = true">ISLE O' HAGS MAP USED FOR IN DEPTH ANALYSIS</button>
          <button @click="showCharacters = true">THAT ONE PICTURE WITH ALL THE CHARACTERS</button>
          <button @click="playSoundEffects">ALL SOUND EFFECTS IN THE GAME IN A ROW FROM SOME YOUTUBE VIDEO</button>
        </div>
        <div class="textbox">
          <p>HOPE YOU LIKED IT! AND DO CHECK OUT THE SOUND EFFECTS. IT'S LIKE 6 AND A HALF MINUTES LONG AND CONTAINS SOME GEMS.</p>
        </div>
        <div class="textbox">
          <p>HAPPY BIRTHDAY 2024 FROM YOUR BRO :)</p>
        </div>
      </div>
    </div>
    <div class="buttonContainer">
      <button v-if="showMap || showCharacters || playSong" @click="back">
        BACK
      </button>
    </div>
  </div>
</template>

<script>
import allSoundEffects from '../assets/allSoundEffects.mp3';

const soundEffects = new Audio(allSoundEffects);
export default {
  name: 'WinScreen',
  data() {
    return {
      showMap: false,
      showCharacters: false,
      playSong: false,
    };
  },
  methods: {
    playSoundEffects: function() {
      this.playSong = true;
      soundEffects.play();
    },
    back: function() {
      soundEffects.pause();
      soundEffects.currentTime = 0;
      this.playSong = false;
      this.showCharacters = false;
      this.showMap = false;
    },
  },
};
</script>

<style scoped>
img {
  margin-top: 10px;
}
p {
  text-align: left;
  padding: 5px;
  padding-left: 20px;
  min-height: 30px;
  margin: 0;
  font-size: 13px;
}
button {
  background-color: rgb(0, 11, 166);
  padding-left: 10px;
  padding-right: 10px;
}
.buttons>button {
  background-color: rgb(209, 22, 22);
  font-size: 16px;
  height: auto;
  margin-bottom: 10px;
  max-width: 100%;
}
.buttons {
  margin-top: 10px;
}
.title {
  padding-top: 10px;
  background-color: rgba(0, 0, 0, 0.15);
  box-shadow: 0 0 5px 5px rgba(0, 0, 0, 0.15);
}
.textbox {
  background-color: rgba(0, 0, 0, .5);
  border-radius: 25px;
  text-align: left;
  margin-top: 10px;
}
.buttonContainer {
  position: absolute;
  bottom: 15px;
  left: 0;
  right: 0;
}
</style>
