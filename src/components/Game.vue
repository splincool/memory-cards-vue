<template>
  <div class="game">
    <div class="info-panel">
      <template v-if="isGameStart">
        <div class="time">Time: {{clock}} </div>
        <div class="moves">Moves: {{moves}}</div>
      </template>
      <template v-else>
        <div class="overlay-box">
          <div 
            class="start-btn" 
            @click="onStart">
              Start
          </div>
        </div>
      </template>
    </div>
    <div class="cards-board">
      <template v-for="(card, index) in cardsArray">
        <Card
          :key="index"
          :_card="card"
          @cardClick="onCardClick"/>
      </template>
    </div>
  </div>
</template>

<script>
import cardsList from '../cards.js'
import Card from './Card.vue'
export default {
  name: 'Game',
  data () {
    return {
      cards: cardsList,
      openCards: [],
      moves: 0,
      clockCount: 0,
      isGameStart: false
    }
  },
  created () {
    this.shuffleCards()
  },
  watch: {
    openCards (_val) {
      if (_val.length === 2) {
        this.timeout = setTimeout(() => {
          this.compareCards()
        }, 500);
      }
    }
  },
  methods: {
    shuffleCards () {
      var randomIndex = null
      var temp = {}
      for (var i = 0; i < this.cards.length; i++) {
        randomIndex = Math.floor(Math.random() * 15)
        temp = this.cards[i]
        this.cards[i] = this.cards[randomIndex]
        this.cards[randomIndex] = temp
      }
    },
    onStart () {
      this.isGameStart = true
    },
    onCardClick (_card) {
      for (let card of this.cards) {
        if (card.id === _card.id) {
          card.isOpen = true
        }
      }
      this.openCards.push(_card)
    },
    compareCards () {
      var cardOne = this.openCards[0]
      var cardTwo = this.openCards[1]
      if (cardOne.name === cardTwo.name) {
        for (let card of this.cards) {
          if (card.name === cardOne.name) {
            card.isGuessed = true
          }
        }
      } else {
        for (let card of this.cards) {
          if ((card.name === cardOne.name) || (card.name === cardTwo.name)) {
            card.isOpen = false
          }
        }
      }
      this.openCards = []
      clearTimeout(this.timeout)
      this.moves += 1
    }
  },
  computed: {
    cardsArray () {
      return this.cards
    },
    clock () {
      this.timeoutClock = setTimeout(() => {
        this.clockCount += 1
      }, 1000);
      return this.clockCount
    }
  },
  components: {
    Card
  }
}
</script>

<style>
  .game {
    width: 430px;
    display: flex;
    flex-direction: column;
    margin: 0 auto;
  }
  .info-panel {
    display: flex;
    justify-content: space-around;
  }
  .cards-board {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    margin: 0 auto;
  }
  .overlay-box {
    position: absolute;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 450px;
    width: 500px;
  }
  .start-btn {
    background: #111;
    color: #fff;
    font-size: 25px;
    font-weight: bold;
    padding: 40px 60px;
    cursor: pointer;
  }
</style>
