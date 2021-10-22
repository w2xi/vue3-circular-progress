<template>
  <div>
    <circle-progress
      :percent="90"
    >
    </circle-progress>

    <vue3-circle-progress
      :percent="10"
      :show-percent="true"
    >
    </vue3-circle-progress>
  </div>
</template>

<script>
import CircleProgress from '@/components/CircleProgressBack.vue'
import Vue3CircleProgress from '@/vue3-circle-progress.vue'

export default {
  name: 'App',
  components: {
    CircleProgress,
    Vue3CircleProgress
  }
}
</script>

<style lang="stylus">
body
  margin: 0
  background: #eee
#app
  font-family Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
</style>
