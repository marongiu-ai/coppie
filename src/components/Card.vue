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
  
  color: white;
  transform: rotateY(180deg);
}

.card-face.is-back {
  
  background-repeat: no-repeat;
  
  background-image: url('/images/back.png');
  border: 2px solid rgba(255,90,0,1);
  
}

.card-face.is-back:hover {
  transform: translate(0%,-4%);
  border: 2px solid rgba(99,195,40,1);
  box-shadow: 10px 10px 17px rgba(255, 255, 0, .9);
  
}
.card-image:hover{
  color: rgba(255, 255, 0, 1);
  box-shadow: 10px 10px 17px rgba(186, 0, 218, .9);
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
