<template>
<!-- 查看未支付的订单-->
  <div>
    <ul class="orderList">
      <li v-for="(item,index) in listData" :key="index">
         <p>{{item.orderId}}</p>
         <p>
           
         </p>
         <p>

         <span v-for="(i,a) in item.shops" :key="a"><img :src="i.img" alt=""></span> 
         </p>
      </li>
    </ul>
  </div>
</template>
<script>
  import  {get,getStorage} from  '../../utils/storage';
    export default {
        name: "orderList",
        data () {
          return {
            listData:[]
          }
        },
      onShow(){
          this.getUserorder()
      },
      methods:{
       async  getUserorder(){
           let openid = await  getStorage('openid');
           let res = await  get('/order/getUserOrder',{
             openid,status:0
           });
           console.log(res)
           const {status,data:{data}} = res
           console.log(data)
           this.listData = data
       }
      }
    }
</script>
<style scoped lang="scss">
@import "./style";
</style>
