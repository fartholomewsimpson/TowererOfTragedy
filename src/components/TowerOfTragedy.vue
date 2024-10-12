<template>
  <div>
    <div class="screen">
      <div class="title" v-if=!started>
        <img alt="Tower of Tragedy" src="../assets/towerOfTragedy.png" />
        <img class="er" alt="er" src="../assets/er.png" />
      </div>
      <div v-else-if="lose">
        <div class="title">
          <img alt="Game Over" src="../assets/gameOver.png" />
        </div>
        <div>
          <div class="textbox score loseScore">
            <p>SCORE: {{ score }}</p>
          </div>
        </div>
      </div>
      <div v-else>
        <div>
          <div class="scoreContainer">
            <div class="textbox score">
              <p>SCORE: {{ score }}</p>
            </div>
          </div>
          <div class="text">
            <div class="textbox">
              <img class="gruntyHead" src="../assets/gruntyHead.png" />
              <p class="question">{{ curQuestion.q.toUpperCase() }}</p>
            </div>
            <div class="textbox answers">
              <p>A. {{ curQuestion.options[0].toUpperCase() }}</p>
            </div>
            <div class="textbox answers">
              <p>B. {{ curQuestion.options[1].toUpperCase() }}</p>
            </div>
            <div class="textbox answers">
              <p>C. {{ curQuestion.options[2].toUpperCase() }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="lives">
      <div>
        <img v-if="mistakes < 1" class="anvil" src="../assets/anvil.png" />
        <img v-else class="anvil hidden" src="../assets/anvil.png" />

        <img v-if="mistakes < 3" class="anvil" src="../assets/anvil.png" />
        <img v-else class="anvil hidden" src="../assets/anvil.png" />

        <img v-if="mistakes < 2" class="anvil" src="../assets/anvil.png" />
        <img v-else class="anvil hidden" src="../assets/anvil.png" />
      </div>
      <div>
        <img v-if="mistakes < 1" class="anvil" src="../assets/mumbo.png" />
        <img v-else class="anvil" src="../assets/anvil.png" />

        <img v-if="mistakes < 3" class="anvil" src="../assets/mumbo.png" />
        <img v-else class="anvil" src="../assets/anvil.png" />

        <img v-if="mistakes < 2" class="anvil" src="../assets/mumbo.png" />
        <img v-else class="anvil" src="../assets/anvil.png" />
      </div>
    </div>
    <div class="buttons">
      <div v-if="!started">
        <button class="buttonStart" @click="start">START</button>
      </div>
      <div v-else-if="lose">
        <button class="buttonStart" @click="reset">RESET</button>
      </div>
      <div v-else>
        <button class="buttonA" @click="answer(0)">A</button>
        <button class="buttonB" @click="answer(1)">B</button>
        <button class="buttonC" @click="answer(2)">C</button>
      </div>
    </div>
  </div>
</template>

<script>
import json from '../assets/questions.json';
import backgroundSong from '../assets/towerOfTragedyMusic.mp3';
import correctSound from '../assets/correct.mp3';
import incorrectSound from '../assets/incorrect.mp3';
import successSound from '../assets/success.mp3';
import failureSound from '../assets/failure.mp3';
import smashSound from '../assets/smash.mp3';
import mumboGoodSound from '../assets/mumboGood.mp3';
import mumboBadSound from '../assets/mumboBad.mp3';
import mumboUmenakaSound from '../assets/mumboUmenaka.mp3';
import gruntyLaughSound from '../assets/gruntyLaugh.mp3';
// import gruntySpeak0 from '../assets/gruntySpeak0.mp3';
// import gruntySpeak1 from '../assets/gruntySpeak1.mp3';
// import gruntySpeak2 from '../assets/gruntySpeak2.mp3';

const song = new Audio(backgroundSong);
const correct = new Audio(correctSound);
const incorrect = new Audio(incorrectSound);
const success = new Audio(successSound);
const failure = new Audio(failureSound);
const smash = new Audio(smashSound);
const mumboGood = new Audio(mumboGoodSound);
const mumboBad = new Audio(mumboBadSound);
const mumboUmenaka = new Audio(mumboUmenakaSound);
const gruntyLaugh = new Audio(gruntyLaughSound);
// const gruntySpeech = [new Audio(gruntySpeak0), new Audio(gruntySpeak1), new Audio(gruntySpeak2)];
export default {
  name: 'TowerOfTragedy',
  data() {
    return {
      questions: [],
      started: false,
      mistakes: 0,
      score: 0,
      lose: false,
    };
  },
  computed: {
    curQuestion() {
      return this.questions[0];
    },
    curQuestionLength() {
      let spaces = 0;
      for (let i = 0; i < this.curQuestion.q.length; i++) {
        if (this.curQuestion.q[i] === ' ') {
          spaces++;
        }
      }
      return spaces;
    },
  },
  mounted() {
    this.initiate();
  },
  methods: {
    initiate: function() {
      this.score = 0;
      this.mistakes = 0;
      this.started = false;
      this.lose = false;
      song.loop = true;
      song.play();
      this.questions = [...json];
      this.questions.forEach(q => {
        q.options = [ ...q.w, q.a ];
        this.scramble(q.options);
      });
      this.scramble(this.questions);
    },
    start: function() {
      this.started = true;
      mumboUmenaka.play();
      // this.speak();
    },
    scramble: function(array) {
      const times = Math.ceil(Math.random() * 3);
      for (let time = 0; time < times; time++) {
        array.forEach((item, i) => {
          const randomIndex = Math.ceil(Math.random() * array.length - 1);
          array[i] = array[randomIndex];
          array[randomIndex] = item;
        });
      }
    },
    answer: function(index) {
      if (this.curQuestion.options[index] === this.curQuestion.a) {
        correct.play();
        mumboGood.play();
        this.score++;
      } else {
        this.mistakes++;
        smash.play();
        if (this.mistakes === 3) {
          this.lose = true;
          song.pause();
          song.currentTime = 0;
          failure.play();
        } else {
          incorrect.play();
          mumboBad.play();
        }
      }
      this.questions.shift();
      if (this.questions.length === 0 && this.mistakes < 3) {
        this.$emit('win');
        success.play();
        mumboUmenaka.play();
      }
      // else {
      //   this.speak();
      // }
    },
    reset: function() {
      if (this.lose === true) {
        gruntyLaugh.play();
        setTimeout(this.initiate, 5000);
      }
    },
    // speak: async function() {
    //   let curIndex = Math.ceil(Math.random() * gruntySpeech.length - 1);
    //   for (let i = 0; i < this.curQuestionLength; i++) {
    //     gruntySpeech[curIndex].pause();
    //     gruntySpeech[curIndex].currentTime = 0;
    //     gruntySpeech[curIndex].play();
    //     const amount = (Math.random() * 300) + 100;
    //     await this.delay(amount);
    //     curIndex += Math.random() > .5 ? 1 : -1;
    //     if (curIndex < 0) {
    //       curIndex = gruntySpeech.length - 1;
    //     } else if (curIndex >= gruntySpeech.length) {
    //       curIndex = 0;
    //     }
    //   }
    // },
    delay: (ms) => new Promise(resolve => setTimeout(resolve, ms)),
  },
}
</script>

<style scoped>
img {
  max-width: 70%;
}
p {
  text-align: left;
  padding: 5px;
  padding-left: 0;
  margin: 0;
  display: inline-block;
  font-size: 13px;
}
.title {
  padding-top: 50px;
  background-color: rgba(0, 0, 0, 0.15);
  box-shadow: 0 0 5px 5px rgba(0, 0, 0, 0.15);
}
.text {
  padding-top: 10px;
  margin-top: 40px;
}
.textbox {
  background-color: rgba(0, 0, 0, .5);
  border-radius: 25px;
  text-align: left;
}
.scoreContainer {
  text-align: end;
  margin-right: 10px;
}
.score {
  display: inline-block;
  margin-top: 10px;
  width: 100px;
  padding-left: 20px;
}
.score>p {
  padding-top: 5px;
  padding-bottom: 5px;
}
.loseScore {
  margin-top: 40px;
}
.question {
  width: 270px;
}
.answers {
  padding-left: 20px;
  min-height: 30px;
  margin-top: 10px;
}
.gruntyHead {
  width: 20%;
  display: inline;
  vertical-align: top;
}
.buttons {
  position: absolute;
  bottom: 15px;
  left: 0;
  right: 0;
}
.buttonStart {
  background-color: rgb(139, 95, 15);
  padding-left: 10px;
  padding-right: 10px;
}
.buttonA {
  background-color: rgb(209, 22, 22);
}
.buttonB {
  background-color: green;
}
.buttonC {
  background-color: blue;
}
.screen {
  min-height: 260px;
  overflow: hidden;
}
.lives {
  position:absolute;
  left: 0;
  right: 0;
  bottom: 120px;
}
.anvil {
  max-width: 25%;
  margin-left: 10px;
  margin-right: 10px;
  max-height: 110px;
}
.hidden {
  opacity: 0;
}
.er {
  vertical-align: top;
  margin-top: 30px;
  animation-duration: 3s;
  animation-name: fallIn;
  max-width: 20%;
}
@keyframes fallIn {
  from {
    translate: 0 -150vh;
    scale: 1 200%;
  }
  to {
    translate: 0 0;
    scale: 1 100%;
  }
}
</style>
