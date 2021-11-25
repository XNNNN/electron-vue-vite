<template>
    <div id="app">
      <toolBar @changePage="changePage"></toolBar>
      <setting :visible="settingVisible"></setting>
    </div>
</template>

<script>
import toolBar from './components/toolBar.vue'
import setting from './components/setting.vue'
import { ref } from "vue";
  export default {
    name: 'app',
    components: {
      toolBar,
      setting
    },
    setup () {
      const settingVisible = ref(false)
      const changePage = (type) => {
        if (type === 0) {
          settingVisible.value = !settingVisible.value
        }
      }
      return {
        changePage,
        settingVisible
      }
    }
  }
</script>

<style>
html {
  background: #1D2D50;
}
body {
  margin: 0;
}
</style>