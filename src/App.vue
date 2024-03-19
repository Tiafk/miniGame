<template>
  <div class="wrapper">
    <radial :colors="colors" :orderColor="orderColor" :currentIndex="currentIndex"  @colorClick="handleClick"/>
    <div class="wrapper-started">
      <div class="start">
        <h3>Rounds: {{ round }}</h3>
        <button @click="startGame" :disabled="gameStarted.value">Start</button>
      </div>
      <div class="levels">
      <h3>Select lvl :</h3>
      <Levels label='Easy' :gameSpeed="1500" @click="handleLevelClick(1500)" id='Easy'/>
      <Levels label='Medium' :gameSpeed="1000" @click="handleLevelClick(1000)" id='Medium'/>
      <Levels label='Hard' :gameSpeed="400" @click="handleLevelClick(400)" id='Hard'/>
    </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import radial from './components/radial.vue'
import Levels from './components/Levels.vue'

const colors = ['blue', 'red', 'green', 'yellow'];
const orderColor = ref([]);
const currentIndex = ref(null);
const gameStarted = ref(false);
const round = ref(0);
const showingColors = ref(false);
const userOrder = ref([]);
const gameSpeed = ref (null)

const handleLevelClick = (speed) => {
  gameSpeed.value = speed
  playSequence(gameSpeed.value);
};

const startGame = () => {
  if (gameSpeed.value == null) {
    console.log(gameStarted.value);
    return false;
  } else {
    gameStarted.value = true;
    showingColors.value = true;
    orderColor.value = [];
    round.value = 0;
    nextRound();
    console.log(gameStarted.value);
  }
};

const nextRound = () => {
  round.value++;
  currentIndex.value = null;
  orderColor.value.push(Math.floor(Math.random() * colors.length) + 1)

  playSequence(orderColor.value);
};


const playSequence = (orderColor) => {
  let i = 0;
  const interval = setInterval(() => {
    currentIndex.value=null;
    setTimeout(() => {
      currentIndex.value = orderColor[i] - 1;
      i++;
    }, 200)
    if (i >= orderColor.length) {
      clearInterval(interval);
      setTimeout(() => {
        showingColors.value = false
        currentIndex.value = null;
      }, 500);
    }
  }, gameSpeed.value);
};


const handleClick = (colorIndex) => {
  console.log(colorIndex);
  if (gameStarted.value && !showingColors.value) {
    userOrder.value.push(colorIndex);
    if (userOrder.value.length === orderColor.value.length) {

      if (userOrder.value.join('') === orderColor.value.join('')) {
        userOrder.value = [];
        console.log(orderColor.value);
        nextRound();
      } else {
        gameOver();
      }
    }
  }
};

const gameOver = () => {
  alert('Game Over!');
  orderColor.value = [];
  userOrder.value = [];
  gameStarted.value = false;
  round.value = 0;
};
</script>

<style lang="scss">
h3 {
  font-family: 'Roboto', sans-serif;
  font-size: 18px;
  letter-spacing: 2px;
  margin-bottom: 10px;
}

.wrapper {
  display: flex;
  width: 100%;
  height: 100vh;
  justify-content: center;
  align-items: center;

  .circle {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 1fr 1fr;
    width: 250px;
    height: 250px;
    border: 4px solid #000;
    border-radius: 50%;
    overflow: hidden;
    margin-right: 60px;
    box-shadow: 6px 5px 2px #fff;

    .item {
      cursor: pointer;
    }

    .blue {
      background-color: #d7ddfc;
      transition: .1s ease;
      &:active {
        background-color: #bcc6f7;
        transition: .1s ease;
      }
      &.active {
        background-color: #bcc6f7;
        transition: .1s ease;
      }
    }

    .red {
      background-color: #ebbaba;
      transition: .1s ease;
      &:active {
        background-color: #ec9e9e;
        transition: .1s ease;
      }
      &.active {
        background-color: #ec9e9e;
        transition: .1s ease;
      }
    }

    .green {
      background-color: hsl(108, 86%, 78%);
      transition: .1s ease;
      &:active {
        background-color: hsl(108, 100%, 66%);
        transition: .1s ease;
      }
      &.active {
        background-color: hsl(108, 100%, 66%);
        transition: .1s ease;
      }
    }

    .yellow {
      background-color: #f5ec99;
      transition: .1s ease;
      &:active {
        background-color: #ffee55;
        transition: .1s ease;
      }
      &.active {
        background-color: #ffee55;
        transition: .1s ease;
      }
    }
  }

  .wrapper-started {
    font-family: 'Roboto', sans-serif;
    
    .start {

      button {
        cursor: pointer;
        padding: 8px 20px;
        border: 0;
        background-color: #72c779;
        font-size: 16px;
        border-radius: 6px;
        color: #fff;
        font-weight: 500;
        margin-bottom: 20px;
        transition: .2s ease;

        &:hover {
          background-color: #5dc265;
          box-shadow: 0 0 5px #000;
          transition: .2s ease;
        }

        &:active {
          transition: .2s ease;
          box-shadow: 0 0 0;
        }
      }
    }

    .levels {
      .item {

        input {
          margin-right: 5px;
        }
        label {
          font-size: 16px;
          font-weight: 500;
        }
      }
    }
  }
}

@media (max-width:440px) {
  .wrapper {
    flex-direction: column;
    gap: 40px;

    .circle {
      margin-right: 0;
    }
  }
}
</style>