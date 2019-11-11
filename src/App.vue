<template>
  <div id="app">
    <select-custom :options="options" :current="current" @update-room-selected="updateCurrentValue"></select-custom>
  </div>
</template>

<script>
import SelectCustom from './components/SelectCustom.vue'

export default {
  name: 'app',
  components: {
    SelectCustom
  },
  data() {
    return {
      options: {
        0: {
          id: 0,
          value: 'Default',
        },
        1: {
          id: 1,
          value: 'Value'
        }
      },
      current: {
        id: 0,
        value: 'Default',
      },
    }
  },
  methods: {
    updateCurrentValue(id, value) {
      this.current = {
        id: id,
        value: value,
      }
    },
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
