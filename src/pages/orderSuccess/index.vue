<template>
  <div>
     <p>
        <icon type="success" size="100"></icon>
     </p>
     <p>购买成功</p>
     <button @click="toIndex()">返回商城</button>
  </div>
</template>
<script>
export default {
     data () {
         return {
         }
     },
     methods: {
         toIndex(){
             wx.switchTab({url: '/pages/index/main'});         
         }
     },
}
</script>
<style lang='scss'>
@import './style.scss';
</style>