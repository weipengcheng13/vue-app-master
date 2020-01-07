<template>
    <briup-fulllayout title="常用地址">
        <van-list>
            <van-cell v-for="item in addresses" :key="item.id" :title="item.province+' '+item.city+' '+item.area+' '+item.address"/>
        </van-list>
        <br>
        <van-button type="default" block @click="toAddressEditHandler">添加</van-button>
    </briup-fulllayout>
</template>

<script>
import {get} from '../../../http/axios'
import {mapState} from 'vuex'
export default {
    data(){
        return{
            addresses:[]
        }
    },
    computed:{ //计算属性
    //将状态机中的user对象中的info对象获取到
        ...mapState("user",["info"])
    },
    created(){
        //调用加载地址信息的方法
        this.loadAddress();
    },
    methods:{
        // 加载当前用户地址信息
        loadAddress(){
            let id = this.info.id;
            let url = "/address/findByCustomerId?id="+id;
            get(url).then((response)=>{
                this.addresses = response.data;
            })
        },
        //跳转到地址编辑页面的处理函数
        toAddressEditHandler(){
            //编程跳转
            this.$router.push("/manager/address_edit");
        }
    }
}
</script>