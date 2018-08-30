<template>
  <div class="game">
    <template v-for="(card, index) in cardsArray">
      <Card
        :key="index"
        :_card="card"
        @cardClick="onCardClick"/>
    </template>
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
      openCards: []
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
    }
  },
  computed: {
    cardsArray () {
      return this.cards
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
    flex-wrap: wrap;
    justify-content: space-between;
    margin: 0 auto;
  }
</style>
