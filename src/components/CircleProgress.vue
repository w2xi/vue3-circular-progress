<template>
  <div class="container">
    <div class="vue-circular-progress">
      <div class="circle">
        <svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="84" height="84" class="circle__svg">
          <circle 
            cx="42" 
            cy="42" 
            r="40" 
            stroke="#f00" 
            stroke-width="2" 
            class="circle__progress circle__progress--path"
          />
          <circle 
            cx="42" 
            cy="42" 
            r="40" 
            stroke="#2BC9A6" 
            stroke-width="4"
            stroke-linecap="round"
            :style="{ 
              strokeDasharray: circumference,
              strokeDashoffset: offset,
              '--initialStroke': circumference,
              '--transitionDuration': 1000,
            }"
            class="circle__progress circle__progress--fill"
          />
        </svg> 
        <div v-if="showPercent" class="current-counter">
          {{ currentPercent + '%' }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import { ref, computed } from '@vue/reactivity'

export default {
  name: 'xs-circle-progress',
  props: {
    radius: {
      type: Number,
      default: 40
    },
    borderWidth: {
      type: Number,
      default: 15
    },
    borderBgWidth: {
      type: Number,
      default: 15
    },
    fillColor: {
      type: String,
      default: '#288feb'
    },
    emptyColor: {
      type: String,
      default: '#dddddd'
    },
     background: {
      type: String,
      default: "none"
    },
    percent: {
      type: Number,
      default: 50
    },
    linecap: {
      type: String,
      default: 'round'
    },
    transition: {
      type: Number,
      default: 400
    },
    showPercent: {
      type: Boolean,
      default: true
    }
  },
  setup(props) {
    const C = Math.PI * 2 * 40
    const circumference = ref(C)
    const currentPercent = ref(0)
    const offset = ref(circumference.value * ( 100 - 0 ) / 100)

    const radius = props.radius

    const svgAttr = {
      version: '1.1',
      xmlns: 'http://www.w3.org/2000/svg',
      viewBox: `${radius / 2} ${radius / 2} ${radius} ${radius}`,
    }

    const circleBgAttr = {
      cx: radius,
      cy: radius,
      r: getCircleBgRadius(),
      stroke: props.emptyColor,
      "stroke-width": props.borderBgWidth,
      fill: props.background,
    }

    const circleFgAttr = computed(() => ({
      cx: radius,
      cy: radius,
      r: getCircleRadius(),
      fill: "none",
      "stroke-width": props.borderWidth,
      "stroke-dasharray": circumference,
      "stroke-dashoffset": offset.value,
      "stroke-linecap": props.linecap,
      ...(props.transition && {
        style: { transition: `stroke-dashoffset ${props.transition}ms` }
      })
    }))
    
    function getCircleBgRadius(){
      return radius + props.borderBgWidth
    }
    
    function getCircleRadius(){
      return radius + props.borderWidth
    }

    function animateValue(to) {
      const step = to - currentPercent.value
      const delay = props.transition / Math.abs(step)
      const counter = setInterval(() => {
        if (step > 0) {
          currentPercent.value += 1
          offset.value = circumference.value * ( 100 - currentPercent.value ) / 100

          if (currentPercent.value >= to) {
            clearInterval(counter)
          }
        } else {
          currentPercent.value -= 1
          offset.value = circumference.value * ( 100 - currentPercent.value ) / 100

          if (currentPercent.value <= to) {
            clearInterval(counter)
          }
        }
      }, delay);
    }

    animateValue(80)

    return {
      svgAttr,
      circumference,
      currentPercent,
      circleBgAttr,
      circleFgAttr,
      offset,
      
    }
  },
}
</script>


<style lang="stylus" scoped>

@import '~@/assets/styles/index.styl'

.container
  height: 84px
  width: 84px
  top: 10px
  left: 100px
  display: flex
  align-items: center
  justify-content: center
  margin: 50px auto

</style>
