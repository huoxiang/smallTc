<template>
  <div class="searchHeadOrder">
       <p @click="showSelect">选择月份</p>
      <div class="MonthSelect" v-show="block">
          <!-- 选择月份的选择框 -->
              <ul>
                  <li @click="select(1)">1月</li>
                  <li @click="select(2)">2月</li>
                  <li @click="select(3)">3月</li>
                  <li @click="select(4)">4月</li>
                  <li @click="select(5)">5月</li>
                  <li @click="select(6)">6月</li>
                  <li @click="select(7)">7月</li>
                  <li @click="select(8)">8月</li>
                  <li @click="select(9)">9月</li>
                  <li @click="select(10)">10月</li>
                  <li @click="select(11)">11月</li>
                  <li @click="select(12)">12月</li>
              </ul>
      </div>
      <div class="list" v-for="(item,index) in list" :key="index">
          <p class="money"><span>订单号:{{item.orderId}}</span><span>{{item.orderStatus==0?'未支付':'已支付'}}</span></p>
          <p class="imgList">
              <img v-for="(i,c) in item.shops" :key='c' :src="i.img" alt="">
          </p>
          <p class="money"> <span>时间:{{item.time}}</span><span> 总计:{{item.money}}元</span></p>
      </div>
      <div>
          <!-- 如果数据为空，显示当前的数据 -->
      </div>
  </div>
</template>
<script>
import {get,getStorage,getLocalTime} from '../../utils/storage'
export default {
     data () {
         return {
            list:[],
            block:false      
         }
     },
     mounted() {
           let Month = new Date().getMonth()+1
           console.log(Month,'当前月份')
         this.getHeaderOrder(Month)
     },
     methods: {
         showSelect(){
             this.block = true
         },
            async select(value){
              this.getHeaderOrder(value)
            },
            async getHeaderOrder(Month){
                //此处接口需要进行更改,如果数据库中的数据过多，可能会造成前端过于卡顿，
                //将接口改为获取当前月份所下订单的接口，只能查看当前年份所有月份的接口
                //调用此接口的用户本身就是团长，所以不从缓存中拿去团长json数据
                let openid = await getStorage('openid')
                let res = await  get('/order/getDateSelect',{
                    Month,
                    headId:openid
                })
                console.log(res.data,'aa')
                if(res.data.code==0){
                    let arr = res.data.data.map(item=>{
                            return {
                                shops:item.shops,
                                time:getLocalTime(item.time),
                                headId:item.headId,
                                orderId:item.orderId,
                                openid:item.openid,
                                money:item.money,
                                orderStatus:item.orderStatus
                            }
                      
                     
                })
                     this.list = arr
                }else{
                    this.list=[]
                }
                this.block=false
               
              }
     }
}
</script> 
<style lang='scss'>
@import './style.scss';
</style>