<template>
    <briup-fulllayout title="订单确认">
    服务地址：
      <van-dropdown-menu>
        <van-dropdown-item v-model="addressId" :options="options" />
      </van-dropdown-menu>
    订单详情：
      <div style="padding:0 2em">
          <p>服务名称：{{$route.query.name}}</p>
          <p>服务名简介：{{$route.query.description}}</p>
          <p>服务价格：{{$route.query.price}}</p>
          <p>服务数量：1</p>
          <p>服务小计：{{$route.query.price*1}}</p>
      </div>
      <div style="position:fixed;bottom:0;width:100%">
        <van-button block type="primary" @click="submitHandler">提交按钮</van-button>
      </div>
    </briup-fulllayout>
</template>

<script>
import {mapState} from 'vuex'
import {get,post, post_obj_array} from '../../../http/axios'
export default {
    data(){
        return{
            addresses:[],
            addressId:0, //服务地址id
            orderLines:[]
        }
    },
    created(){
        this.loadAddress();
        //初始化订单项，将我们购买的产品放入到购物车
        let orderLine = {
            number:1,
            price:this.$route.query.price,
            productId:this.$route.query.id
        };
        this.orderLines.push(orderLine);
    },
    computed:{
        ...mapState("user",["info"]),
        options:function(){
           return this.addresses.map(item=>{
                return{
                    text:item.province+' '+item.city+' '+item.area+' '+item.address,
                    value:item.id
                }
            })
        }
    },
    methods:{
        loadAddress(){
            let id = this.info.id;
            let url = "/address/findByCustomerId?id="+id;
            get(url).then((response)=>{
                this.addresses = response.data;
                this.addressId = this.addresses[0].id;
            })
        },
        submitHandler(){
            let url = "/order/save";
            let data = {
                customerId:this.info.id,
                addressId:this.addressId,
                orderLines:this.orderLines
            }
            post_obj_array(url,data).then(response=>{
                this.$toast("提交成功");
                this.$router.push("/manager/order");
            })
        }
    }
}
</script>