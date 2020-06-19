<template>
  <div
    class="itemSelection-container"
    id="itemSelectionContainer"
    @mousedown="mousedown"
  >
    <div
      class="itemSelection-item"
      id="img_1"
      value="1"
    >1</div>
    <div
      class="itemSelection-item"
      id="blk_1"
      value="2"
    >2</div>
    <div
      class="itemSelection-item"
      id="vid_1"
      value="3"
    >3</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectBox: null,
      startX: null,
      startY: null,
      initx: null,
      scrollX: null,
      scrollY: null,
      inity: null
    }
  },
  props: {
  },
  mounted() {
  },
  methods: {
    clearBubble(e) {
      if (e.stopPropagation) { e.stopPropagation() } else { e.cancelBubble = true }
      if (e.preventDefault) { e.preventDefault() } else { e.returnValue = false }
    },
    addSelected() {
      let elArray = document.getElementsByClassName('itemSelection-item')
      for (let i = 0; i < elArray.length; i++) {
        let itemX_pos = elArray[i].offsetWidth + elArray[i].offsetLeft
        let itemY_pos = elArray[i].offsetHeight + elArray[i].offsetTop
        let condition1 = itemX_pos > this.left
        let condition2 = itemY_pos > this.top
        let condition3 = elArray[i].offsetLeft < (this.left + this.width)
        let condition4 = elArray[i].offsetTop < (this.top + this.height)
        if (condition1 && condition2 && condition3 && condition4) {
          elArray[i].classList.add('item-selected')
        } else {
          elArray[i].classList.remove('item-selected')
        }
      }
    },
    clearSelected() {
      let elArray = document.getElementsByClassName('itemSelection-item')
      for (let i = 0; i < elArray.length; i++) {
        let list = elArray[i].classList
        if (list.contains('item-selected')) {
          list.remove('item-selected')
        }
      }
    },
    clearSelectedBox() {
      if (this.selectBox) {
        this.selectBox.parentNode.removeChild(this.selectBox)
      }
    },
    consoleSelected() {
      let elArray = document.getElementsByClassName('item-selected')
      let list = []
      for (let i = 0; i < elArray.length; i++) {
        list[i] = {"id": elArray[i].getAttribute('id'),"value": elArray[i].getAttribute('value')}
      }
      if(list.length!==0){
        console.table(list)
      }
    },
    mousedown(e) {
      // 初始化
      // this.clearSelectedBox()
      this.clearSelected()
      // 创建框选 div
      this.selectBox = document.createElement('div')
      this.selectBox.className = 'itemSelection-selectBox'
      // 添加框选 div 到 body 并获取滚动距离
      document.body.appendChild(this.selectBox)
      this.scrollX = document.documentElement.scrollLeft || document.body.scrollLeft
      this.scrollY = document.documentElement.scrollTop || document.body.scrollTop
      // 初始化框选 div 位置信息
      this.startX = e.x + this.scrollX || e.clientX + this.scrollX
      this.startY = e.y + this.scrollY || e.clientY + this.scrollY
      this.selectBox.style.cssText = `left:${this.startX}px;top:${this.startY}px`
      // 清除模型事件
      this.clearBubble(e)
      // 监听鼠标事件
      document.getElementById('itemSelectionContainer').addEventListener('mousemove', this.mousemove)
      document.body.addEventListener('mouseup', this.mouseup)
    },
    mousemove(e) {
      // 初始化框选 div 可见
      this.selectBox.style.cssText += 'display: block;'
      // 初始化框选 div 宽、高
      this.initx = e.x + this.scrollX || e.clientX + this.scrollX
      this.inity = e.y + this.scrollY || e.clientY + this.scrollY
      // 计算框选 div 的属性值
      this.left = Math.min(this.initx, this.startX)
      this.top = Math.min(this.inity, this.startY)
      this.width = Math.abs(this.initx - this.startX)
      this.height = Math.abs(this.inity - this.startY)
      this.selectBox.style.left = `${this.left}px`
      this.selectBox.style.top = `${this.top}px`
      this.selectBox.style.width = `${this.width}px`
      this.selectBox.style.height = `${this.height}px`

      // console.log(this.height);
      // 超出边界则取消

      // 判断并添加/清除 selected
      this.addSelected()
      // 清除模型事件
      this.clearBubble(e)
    },
    mouseup(e) {
      // 清除移动监听
      document.getElementById('itemSelectionContainer').removeEventListener('mousemove', this.mousemove)
      // 清除选框
      this.clearSelectedBox()
      // 清除模型事件
      this.clearBubble(e)
      // 打印选中元素
      this.consoleSelected()
    },
  }
}
</script>
<style  scoped>
.itemSelection-container {
  width: 100%;
  overflow: hidden;
  box-sizing: border-box;
  padding: 200px 0 200px 0;
  border: 1px solid red;
  background-color: #fff;
  display: flex;
  justify-content: center;
}
.itemSelection-container .itemSelection-item {
  background-color: #eee;
  flex: 0 0 1;
  height: 50px;
  width: 50px;
  margin-right: 20px;
  /* opacity: 0.2; */
  box-sizing: border-box;
  /* 优化一下挤压内容，消除抖动 */
  border: 1px solid transparent;
}
.itemSelection-container .itemSelection-item.item-selected {
  border: 1px dashed black;
}
</style>

<style>
.itemSelection-selectBox {
  position: absolute;
  display: none;
  width: 0px;
  height: 0px;
  padding: 0px;
  margin: 0px;
  /* border: 1px dashed #0099ff; */
  background-color: #c3d5ed;
  opacity: 0.5;
  filter: alpha(opacity=50);
  font-size: 0px;
  z-index: 99999;
}
</style>