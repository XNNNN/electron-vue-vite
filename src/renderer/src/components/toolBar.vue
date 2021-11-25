<template>
  <div ref="toolBar" class="tool_bar">
    <div ref="addSymbol" @click="showFunction" class="add_symbol">
      <div class="row" ></div>
      <div class="column"></div>
    </div>
    <div class="function_area">
      <div ref="setting" class="setting common_icon" @mouseover="scaleFnIcon(0)" @click="goFunction(0)">
      </div>
      <div ref="add" class="add common_icon"  @mouseover="scaleFnIcon(1)" @click="goFunction(1)">
      </div>
      <div ref="statistics" class="statistics common_icon" @mouseover="scaleFnIcon(2)" @click="goFunction(2)">
      </div>
    </div>
  </div>
</template>
<script>
import anime from 'animejs';
import { onMounted, ref } from 'vue'
export default {
  setup (props, context) {
    const toolBar = ref(null)
    const addSymbol = ref(null)
    const setting = ref(null)
    const add = ref(null)
    const statistics = ref(null)
    const isShowListFn = ref(false) //是否展示功能
    const fnArray = [setting, add, statistics]
    const start = () => { // 初始化动画
      anime({
        targets: toolBar.value,
        translateY: [
          { value: -100, duration: 1500 }
        ],
        scaleY: [
          { value: [1.75, 1], duration: 1500 },
        ],
        easing: 'easeOutElastic(1, .8)'
      });
    }
    const showFunction = () => { // 点击加号动画旋转
      isShowListFn.value = !isShowListFn.value
      anime({
        targets: addSymbol.value,
        rotate: isShowListFn.value === true ? '45deg' : '90deg',
        begin: function() {
          showMore()
        }
      });
    }
    const showMore = () => { // 点击加号展示功能菜单
      if (isShowListFn.value) {
        anime({
          targets: setting.value,
          scaleY: [
            { value: [0, 1], duration: 1500 },
          ],
          scaleX: [
            { value: [0, 1], duration: 1500 },
          ],
          translateX: [0, -45],
          translateY: [0, -45],
        });
        anime({
          targets: add.value,
          scaleY: [
            { value: [0, 1], duration: 1500 },
          ],
          scaleX: [
            { value: [0, 1], duration: 1500 },
          ],
          translateY: [0, -70],
        });
        anime({
          targets: statistics.value,
          scaleY: [
            { value: [0, 1], duration: 1500 },
          ],
          scaleX: [
            { value: [0, 1], duration: 1500 },
          ],
          translateX: [0, -70],
        });
      } else {
        anime({
          targets: [statistics.value, add.value, setting.value],
          scaleY: [
            { value: [1, 0], duration: 1500 },
          ],
          scaleX: [
            { value: [1, 0], duration: 1500 },
          ],
          scale: 1
        });
      }
    }
    const scaleFnIcon = (type) => {
      for (let index = 0; index < fnArray.length; index++) {
        if (type === index) {
          anime({
            targets: fnArray[type].value,
            scale: 1.5
          });
        } else {
          anime({
            targets: fnArray[index].value,
            scale: 0.8
          });
        }
      }
    }
    const goFunction = (type) => {
      context.emit('changePage', type)
    }
    onMounted(
      () => {
        start()
      }
    )
    return {
      toolBar,
      addSymbol,
      start,
      showFunction,
      isShowListFn,
      setting,
      showMore,
      add,
      statistics,
      scaleFnIcon,
      goFunction
    }
  }
}
</script>
<style lang="scss" scoped>
.tool_bar {
  position: fixed;
  bottom: -80px;
  right: 20px;
  .add_symbol {
    cursor: pointer;
    display: flex;
    width: 50px;
    background: #1E5F74;
    height: 50px;
    border-radius: 50px;
    justify-content: space-around;
    align-items: center;
    position: relative;
    z-index: 10;
    .row {
      width: 8px;
      height: 35px;
      background: #FCDAB7;
      border-radius: 8px;
      position: absolute;
    }
    .column {
      height: 8px;
      width: 35px;
      background: #FCDAB7;
      border-radius: 8px;
      position: absolute;
    }
  }
  .function_area {
    position: absolute;
    z-index: 5;
    top:5px;
    left: 5px;
    .common_icon {
      position: absolute;
      top: 0;
      width: 35px;
      height: 35px;
    }
    .setting {
      background: url('../assets/iconPNG/setting.png') no-repeat center center;
      background-size: contain;
    }
    .add {
      background: url('../assets/iconPNG/add.png')  no-repeat center center;
      background-size: contain;
    }
    .statistics {
      background: url('../assets/iconPNG/statistics.png') no-repeat center center;
      background-size: contain;
    }
  }
}
</style>