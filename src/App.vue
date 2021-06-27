<script>
import { ref, watch } from 'vue'
import createDeck from './features/createDeck'
import createGame from './features/createGame'
import { launchConfetti } from './utilities/confetti'
import GameBoard from './components/GameBoard'
import NewGameButton from './components/NewGameButton'
import halloweenDeck from './data/halloweenDeck.json'


export default {
  name: 'App',
  components: {
    GameBoard,
    NewGameButton
  },
  setup() {
    const { cardList } = createDeck(halloweenDeck)
    const {
      newPlayer,
      startGame,
      restartGame,
      matchesFound,
      status
    } = createGame(cardList)
    const userSelection = ref([])

    const startNewGame = () => {
      if (newPlayer) {
        startGame()
      } else {
        restartGame()
      }
    }

    const flipCard = payload => {
      cardList.value[payload.position].visible = true

      if (userSelection.value[0]) {
        if (
          userSelection.value[0].position === payload.position &&
          userSelection.value[0].faceValue === payload.faceValue
        ) {
          return
        } else {
          userSelection.value[1] = payload
        }
      } else {
        userSelection.value[0] = payload
      }
    }

    watch(matchesFound, currentValue => {
      if (currentValue === 8) {
        launchConfetti()
      }
    })

    watch(
      userSelection,
      currentValue => {
        if (currentValue.length === 2) {
          const cardOne = currentValue[0]
          const cardTwo = currentValue[1]

          if (cardOne.faceValue === cardTwo.faceValue) {
            cardList.value[cardOne.position].matched = true
            cardList.value[cardTwo.position].matched = true
          } else {
            setTimeout(() => {
              cardList.value[cardOne.position].visible = false
              cardList.value[cardTwo.position].visible = false
            }, 2000)
          }

          userSelection.value.length = 0
        }
      },
      { deep: true }
    )

    return {
      cardList,
      flipCard,
      userSelection,
      status,
      startNewGame,
      newPlayer
    }
  }
}
</script>

<template>
<img src="/images/scegli.png"
 alt="Peek-a-Vue" class="title"/>


  <NewGameButton :newPlayer="newPlayer" @start-new-game="startNewGame" />
  <GameBoard :cardList="cardList" :status="status" @flip-card="flipCard" />
  
</template>

<style>
html,
body {
  margin: 0;
  padding: 0;
  
}

html {
  background-image: linear-gradient(rgb(0, 33, 0),green, brown, green,rgb(0, 33, 0));;
  background-color: #00070c;
}

h1 {
  margin-top: 0;
}



a {
  color: white;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  color: #fff;
  padding: 60px 0;
}

.status {
  font-family: 'Titillium Web', sans-serif;
  font-size: 18px;
  text-transform: uppercase;
}

.game-board {
  display: grid;
  grid-template-columns: repeat(3, 130px);
  grid-template-rows: repeat(6, 180px);
  grid-column-gap: 6px;
  grid-row-gap: 12px;
  justify-content: center;
}

@media screen and (max-width: 499px) {
  .game-board {
    grid-template-columns: repeat(3, 130px);
    grid-template-rows: repeat(6, 180px);
    grid-column-gap: 6px;
    grid-row-gap: 12px;
  }
  .card-face.is-back {
  
  background-repeat: no-repeat;
  
  background-image: url('/images/backreduced.png');
  }
  
}
@media screen and (max-width: 499px) {
  .game-board {
    grid-template-columns: repeat(3, 130px);
    grid-template-rows: repeat(6, 180px);
    grid-column-gap: 6px;
    grid-row-gap: 12px;

  }


}
@media screen and (min-width: 500px) and (max-width: 899px) {
  .game-board {
    grid-template-columns: repeat(5, 130px);
    grid-template-rows: repeat(4, 180px);
    grid-column-gap: 6px;
    grid-row-gap: 12px;
  }
  .card-face.is-back {
  
  background-repeat: no-repeat;
  
  background-image: url('/images/backreduced.png');
  }
  

  
}
@media screen and  (min-width: 500px) and (max-width: 899px) {
  .game-board {
    grid-template-columns: repeat(5, 130px);
    grid-template-rows: repeat(4, 180px);
    grid-column-gap: 6px;
    grid-row-gap: 12px;

  }


}


@media screen and (min-width: 900px) {
  .game-board {
    grid-template-columns: repeat(5, 218px);
    grid-template-rows: repeat(4, 303px);
    grid-column-gap: 12px;
    grid-row-gap: 12px;
  }
}

.title{
   width:40%;
  
  padding-bottom: 10px;

}

.shuffle-card-move {
  transition: transform 0.8s ease-in;
}
</style>
