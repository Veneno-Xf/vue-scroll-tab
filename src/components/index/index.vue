<template>
  <div class = "tabContain">
    <Tab :tabData='tabData'></Tab>
  </div>
</template>
<script type="text/ecmascript-6">
import Tab from 'components/tab/tab'
export default {
  data () {
    return {
      tabData: {
        Data: [{
          name: '页面1',
          src: '../../static/images/page1.png'
         },
         {
           name: '页面2',
           src: '../../static/images/page2.png'
         },
         {
           name: '页面3',
           src: '../../static/images/page3.png'
         },
         {
           name: '页面4',
           src: '../../static/images/page4.png'
         },
         {
           name: '页面5',
           src: '../../static/images/page5.png'
         },
         {
           name: '页面6',
           src: '../../static/images/page6.png'
         },
         {
           name: '页面7',
           src: '../../static/images/page7.png'
         }]

       }
    }
  },
  components: {
    Tab
  }
}
</script>
<style lang="stylus" rel="stylesheet/stylus">

</style>
