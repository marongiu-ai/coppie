<script>
import { computed } from 'vue'

export default {
  props: {
    matched: {
      type: Boolean,
      default: false
    },
    position: {
      type: Number,
      required: true
    },
    value: {
      type: String,
      required: true
    },
    visible: {
      type: Boolean,
      default: false
    }
  },
  setup(props, context) {
    const flippedStyles = computed(() => {
      if (props.visible) {
        return 'is-flipped'
      }
    })

    const selectCard = () => {
      context.emit('select-card', {
        position: props.position,
        faceValue: props.value
      })
    }

    return {
      flippedStyles,
      selectCard
    }
  }
}
</script>

<template>
  <div class="card" :class="flippedStyles" @click="selectCard">
    <div class="card-face is-front">
      <img
        class="card-image"
        :src="`/images/${value}.png`"
        :alt="value"
      />
      <img v-if="matched" src="/images/checkmark.svg" class="icon-checkmark" />
    </div>
    <div class="card-face is-back">
    <img class="spider" src="https://raw.githubusercontent.com/WebDevSimplified/Mix-Or-Match/master/Assets/Images/Spider.png" >
    <img class="cob-web cob-web-top-left" src="https://raw.githubusercontent.com/WebDevSimplified/Mix-Or-Match/master/Assets/Images/Cobweb.png">
        <img class="cob-web cob-web-top-right" src="https://raw.githubusercontent.com/WebDevSimplified/Mix-Or-Match/master/Assets/Images/Cobweb.png">
        <img class="cob-web cob-web-bottom-left" src="https://raw.githubusercontent.com/WebDevSimplified/Mix-Or-Match/master/Assets/Images/Cobweb.png">
        <img class="cob-web cob-web-bottom-right" src="https://raw.githubusercontent.com/WebDevSimplified/Mix-Or-Match/master/Assets/Images/Cobweb.png">
    </div>
  </div>
</template>

<style>
.card {
  position: relative;
  transition: 0.5s transform ease-in;
  transform-style: preserve-3d;
}

.card.is-flipped {
  transform: rotateY(180deg);
}

.card-face {
  width: 100%;
  height: 100%;
  position: absolute;
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  backface-visibility: hidden;
}

.card-face.is-front {
  background-image: url('/images/card-bg.png');
  color: white;
  transform: rotateY(180deg);
}

.card-face.is-back {
  
  background-repeat: no-repeat;
  background-color: black;
  align-self: flex-start;
  border-color: #FF6D00;
  
  
}

.cob-web {
  position: absolute;
  width: 47px;
  height: 47px;
  transition: width 100ms ease-in-out, height 100ms ease-in-out;
}

.card-face:hover .cob-web {
  width: 52px;
  height: 52px;
}

.cob-web-top-left {
  transform: rotate(270deg);
  top: 0;
  left: 0;
}

.cob-web-top-right {
  top: 0;
  right: 0;
}

.cob-web-bottom-left {
  transform: rotate(180deg);
  bottom: 0;
  left: 0;
}

.cob-web-bottom-right {
  transform: rotate(90deg);
  bottom: 0;
  right: 0;
}

.spider {
  align-self: flex-start;
  transform: translateY(-10px);
  transition: transform 100ms ease-in-out;
}

.card-face is-back:hover .spider {
  transform: translateY(0)
}
.card-image {
  max-width: 100%;
}

.icon-checkmark {
  position: absolute;
  right: 5px;
  bottom: 5px;
}
</style>
