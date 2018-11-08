<template>
  <div class="tabTop">
    <div class="top-wrapper" ref="topWrapper">
      <ul  class="top-group" ref="topGroup">
        <li class="topItem" v-for="(item,index) in tabData.Data" :key = 'index' :class="{'current':currentIndex===index}"
        @click="selectTab(index,$event)" ref="topList">
          {{item.name}}
        </li>
      </ul>
    </div>
    <div class="main-wrapper"  ref="mainWrapper">
      <ul  class="main-group" ref="mainGroup">
        <li class="mainItem" v-for="(item,index) in tabData.Data" :key = 'index' ref="mainList">
            <img :src="item.src">
        </li>
      </ul>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'

  export default {
      props: {
        tabData: {
          type: Object
        }
      },
      data() {
        return {
          listWidth: [],
          scrollX: 0
        }
      },
      mounted() {
        this.$nextTick(() => {
          this._getWidth()
          this._initScroll()
          this._calculateWidth()
        })
      },
      computed: {
        currentIndex() {
          for (let i = 0; i < this.listWidth.length; i++) {
            let width1 = this.listWidth[i]
            let width2 = this.listWidth[i + 1]
            if (!width2 || (this.scrollX >= width1 && this.scrollX < width2)) {
              this._followScroll(i)
              return i
            }
          }
          return 0
        }
      },
      methods: {
        selectTab(index, event) {
          if (!event._constructed) {
            return
          }
          let mainList = this.$refs.mainList
          let el = mainList[index]
          this.mainScroll.scrollToElement(el, 300)
        },
        _initScroll() {
          this.topScroll = new BScroll(this.$refs.topWrapper, {
            startX: 0,
            click: true,
            scrollX: true,
            scrollY: false,
            eventPassthrough: 'vertical'
          })
          this.mainScroll = new BScroll(this.$refs.mainWrapper, {
            scrollX: true,
            scrollY: false,
            momentum: false,
            snap: true,
            snapLoop: this.loop,
            snapThreshold: 0.3,
            snapSpeed: 400,
            probeType: 3
          })
          this.mainScroll.on('scroll', (pos) => {
            // 判断滑动方向，避免下拉时分类高亮错误（如第一分类商品数量为1时，下拉使得第二分类高亮）
            if (pos.x <= 0) {
              this.scrollX = Math.abs(Math.round(pos.x))
            }
          })
        },
        _calculateWidth() {
          let mainList = this.$refs.mainList
          let width = 0
          this.listWidth.push(width)
          for (let i = 0; i < mainList.length; i++) {
            let item = mainList[i]
            width += item.clientWidth
            this.listWidth.push(width)
          }
        },
        _followScroll(index) {
          let topList = this.$refs.topList
          let el = topList[index]
          this.topScroll.scrollToElement(el, 300, true, 0)
        },
            _getWidth() {
              this.children = this.$refs.topGroup.children
              let width = 0
              for (let i = 0; i < this.children.length; i++) {
                let item = this.children[i]
                width += item.clientWidth + 1
              }
              this.$refs.topGroup.style.width = width + 'px'

              let thatWidth = this.$refs.mainWrapper.clientWidth
              this.childrensc = this.$refs.mainGroup.children
              let widthm = 0
              for (let i = 0; i < this.childrensc.length; i++) {
                let items = this.childrensc[i]
                items.style.width = thatWidth + 'px'
                widthm += items.clientWidth
              }
              this.$refs.mainGroup.style.width = widthm + 'px'
            }
      }
    }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .top-wrapper
    width: 100%
    height: 100%
    box-sizing: border-box
    overflow: hidden
    .topItem
      width: 25vw
      height: 40px
      line-height: 40px
      text-align: center
      display: inline-block
      text-align: center
      position: relative
    .current
      border-bottom: 2px solid #5d50ad
  .main-wrapper
    background: #ffffff
    box-sizing: border-box
    overflow: hidden
    .mainItem
      background: #ffffff
      display: inline-block
      img
        width: 100%
</style>
