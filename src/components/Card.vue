<template>
  <div
    class="card"
    :class="{'guessed' : _card.isGuessed}"
    @click="$emit('cardClick', _card)">
    <div v-if="!_card.isOpen"
      class="back">
    </div>
    <div
      v-else
      class="front">
      {{_card.name}}
    </div>
  </div>
</template>

<script>
export default {
  name: 'Card',
  props: {
    _card: Object,
    _isOpen: Boolean
  }
}
</script>

<style>
.card {
  width: 100px;
  height: 100px;
  background: red;
  margin: 5px 0;
  cursor: pointer;
}
.card .back {
  background: lightgreen;
  height: 100%;
}
.card .front {
  background: lightsalmon;
  height: 100%;
}
</style>
