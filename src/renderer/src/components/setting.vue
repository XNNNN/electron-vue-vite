<template>
  <div ref="backgroundAnimate" class="background_animate"></div>
  <div ref="settingContent" class="setting_content">
    我是内容
  </div>
</template>
<script>
import anime from 'animejs';
import { ref, toRefs, watch } from '@vue/runtime-core'
export default {
  props: {
    visible: {
      type: Boolean,
      default: false
    }
  },
  setup (props) {
    const { visible } = toRefs(props)
    const backgroundAnimate = ref(null)
    const settingContent = ref(null)
    watch (visible, (newVal) => {
      backgroundScale(newVal)
    })
    const backgroundScale = (isShow) => {
      anime({
        targets: backgroundAnimate.value,
        borderRadius: '100%',
        scale: [
          { 
            value: isShow ? [0, 45] : [45, 0],
            duration: 1100,
            easing: 'easeInOutCubic',
            changeComplete: () => {
              anime({
                targets: settingContent
              });
            }
          },
        ]
      });
    }
    return {
      backgroundAnimate,
      backgroundScale,
      settingContent
    }
  }
}
</script>
<style lang="scss" scoped>
.background_animate {
  position: fixed;
  top: 50%;
  left: -25px;
  z-index: -1;
  transform: translate(-50%, -50%);
  width: 25px;
  height: 25px;
  border-radius: 0 25px 25px 0;
  background: blanchedalmond;
}
.setting_content {
  // opacity: 0;
}
</style>