<template>
  <div class="demo">
    <circle-progress
      :size="120"
    ></circle-progress>

    <circle-progress
      :show-percent="true"
      :percent="10"
      :size="150"
      fill-color="#67C23A"
      empty-color="#ddd"
      :border-bg-width="10"
    >
    </circle-progress>

    <circle-progress
      :show-percent="true"
      :percent="50"
      fill-color="#E6A23C"
      empty-color="#999"
      :border-bg-width="8"
    >
      <template #content="row">
         {{ row.percent + '%' }}
      </template>
    </circle-progress>

    <circle-progress
      :show-percent="true"
      :percent="50"
      fill-color="#E6A23C"
      empty-color="#000"
      :border-bg-width="8"
    >
      <template #content>
        <span style="color: #fff; fontSize: 20px">Hello</span>
      </template>
    </circle-progress>
  </div>
</template>

<script>
import CircleProgress from '@/components/CircleProgress.vue'
import { ref } from '@vue/reactivity'

export default {
  name: 'App',
  components: {
    CircleProgress,
  },

  setup(){
    const percent = ref(30)

    return {
      percent
    }
  },
}
</script>

<style lang="stylus">
html,
body,
#app,
.demo
  height: 100%
  width: 100%
  margin: 0

.demo
  display: flex
  justify-content: center
  align-items: center
  background: #3e423a
  
.vue-circle-progress
  margin-left: 20px 
  font-size: 20px
</style>
