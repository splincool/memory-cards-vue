<template>
  <div class="card-wrapper">
    <div
      v-if="!_card.isGuessed"
      class="card">
      <div v-if="!_card.isOpen"
        @click="$emit('cardClick', _card)"
        class="back">
      </div>
      <div
        v-else
        class="front"
        :style="{'background': _card.color}">
        {{_card.name}}
      </div>
    </div>
    <div v-else class="empty-card"></div>
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
.card-wrapper {
  width: 100px;
  height: 100px;
  margin: 5px 0;
}
.card {
  height: 100%;
  cursor: pointer;
}
.card .back {
  background: #a593c6;
  height: 100%;
  border-radius: 6px;
}
.card .front {
  background: lightsalmon;
  height: 100%;
  border-radius: 6px;
  color: #fff;
  display: flex;
  justify-content: center;
  align-items: center;
  font-weight: bold;
}
@media only screen and (max-width: 414px) {
  .card-wrapper {
    width: 95px;
    height: 95px;
  }
}
@media only screen and (max-width: 375px) {
  .card-wrapper {
    width: 83px;
    height: 83px;
  }
}
@media only screen and (max-width: 320px) {
  .card-wrapper {
    width: 70px;
    height: 70px;
  }
}
</style>
