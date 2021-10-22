<template>
  <div class="container">
    <div class="vue-circular-progress">
      <div class="circle" v-bind="wrapAttr">
        <svg xmlns="http://www.w3.org/2000/svg" v-bind="svgAttr" class="circle__svg">
          <circle 
            v-bind="circleBgAttr"
            class="circle__progress circle__progress--path"
          />
          <circle 
            v-bind="circleFgAttr"
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
import { watch } from '@vue/runtime-core';

export default {
  name: 'xs-circle-progress',
  props: {
    size: {
      type: Number,
      default: 100
    },
    borderWidth: {
      type: Number,
      default: 10
    },
    borderBgWidth: {
      type: Number,
      default: 5
    },
    fillColor: {
      type: String,
      default: '#f00'
    },
    emptyColor: {
      type: String,
      default: '#00f'
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
      default: 1000
    },
    showPercent: {
      type: Boolean,
      default: true
    }
  },
  setup(props) {
    const currentPercent = ref(0);

    const circleRadiusBg = () => {
      let value = (props.size - props.borderBgWidth) * 0.5;
      if (props.borderWidth > props.borderBgWidth) {
        value -= (props.borderWidth - props.borderBgWidth) * 0.5;
      }
      return value;
    };

    const circleRadiusFg = () => {
      let value = (props.size - props.borderWidth) * 0.5;
      if (props.borderBgWidth > props.borderWidth) {
        value -= (props.borderBgWidth - props.borderWidth) * 0.5;
      }
      return value;
    };

    const circumference = 2 * Math.PI * circleRadiusFg();
    const offset = ref(2 * Math.PI * circleRadiusFg());

    const wrapStyle = {
      height: props.size + "px",
      width: props.size + "px",
      position: "relative"
    };

    const wrapAttr = {
      class: props.class,
      style: wrapStyle
    };

    const svgAttr = {
      // style: {
      //   transform: "rotate(-90deg)",
      //   overflow: "visible"
      // },
      xmlns: "http://www.w3.org/2000/svg",
      viewBox: `${props.size / 2} ${props.size / 2} ${props.size} ${props.size}`
    };

    const circleBgAttr = {
      cx: props.size,
      cy: props.size,
      r: circleRadiusBg(),
      stroke: props.emptyColor,
      "stroke-width": props.borderBgWidth,
      fill: props.background,
    };

    const circleFgAttr = computed(() => ({
      cx: props.size,
      cy: props.size,
      r: circleRadiusFg(),
      fill: "none",
      "stroke-width": props.borderWidth,
      "stroke-dasharray": circumference,
      "stroke-dashoffset": offset.value,
      "stroke-linecap": props.linecap,
      stroke: props.fillColor,
      // ...(props.transition && {
      //   style: { transition: `stroke-dashoffset ${props.transition}ms` }
      // })
    }));

    function updatePercent() {
      const circumference = 2 * Math.PI * circleRadiusFg();
      offset.value = circumference - (circumference * props.percent) / 100;
      const newPercent = Math.round(100 - (100 / circumference) * offset.value);
      animateValue(newPercent);
    }

    function animateValue(to) {
      const step = to - currentPercent.value;
      const delay = props.transition / Math.abs(step);
      const counter = setInterval(() => {
        if (step > 0) {
          currentPercent.value += 1;
          if (currentPercent.value >= to) {
            clearInterval(counter);
          }
        } else {
          currentPercent.value -= 1;
          if (currentPercent.value <= to) {
            clearInterval(counter);
          }
        }
      }, delay);
    }

    animateValue(props.percent)

    watch(
      () => props.percent,
      (val) => {
        updatePercent()
      }
    )

    return {
      wrapAttr,
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
