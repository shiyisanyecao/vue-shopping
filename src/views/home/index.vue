<template>
<div class="ui-app with-header">
    <!-- 欢迎动画组件 -->
    <cover :show="loadding.show"></cover>
    <!-- 遮罩层组件 -->
    <mask :show="mask"></mask>
    <!--左侧菜单组件-->
    <menuLeft :show="menu.show" :list="menu.list"></menuLeft>
    <!--头部组件-->
    <head-module></head-module>
    <!--首页登陆组件-->
    <login-module :loginpic="loginpic"></login-module>
    <!--中心数据组件-->
    <views-module :goods="goodslist" :markets="marketslist"></views-module>
    <!--广告组件-->
    <down-module></down-module>
</div>
</template>

<script>
//公用组件
import Cover from '../../components/cover.vue'//欢迎动画组件
import Mask from '../../components/mask.vue'//遮罩层组件
import Menuleft from '../../components/menuleft.vue'//左侧菜单组件
import HeadModule from '../../components/head.vue'//头部组件

//局部业务组件
import LoginModule from '../../views/home/login.vue'//首页登陆
import DownModule from '../../views/home/down.vue'//广告组件
import ViewsModule from '../../views/home/views.vue'//中心数据组件
export default {
    data() {
        return {
            mask: false,
            loginpic:"",
            loadding: {
                show: true
            },
            menu:{
                show: false,
                list:[]
            }
        }
    },
    components: {
        Cover,
        Mask,
        Menuleft,
        HeadModule,
        LoginModule,
        DownModule,
        ViewsModule
    }
    methods: {
        //请求列表全部数据
        getAjax(transition){
            const self = this
            let successCallback =(response) => {
                const jsondata = response.data
                self.$route.router.app.loading = false
                self.loadding.show = false
                if(jsondata&&jsondata.code==0){
                    //实时异步队列更新数据
                    transition.next({
                        loginpic:jsondata.data.advertisement,
                        marketslist:jsondata.data.markets,
                        goodslist:jsondata.data.goods,
                        menu:{
                            show:false,
                            list:jsondata.data.goods
                        },
                    })
                }
            }
            let errorCallback = (json)=> {
                //console.log(json)
            }
            let data = {
                id:'001'
            }
            let options ={
                name:'lei'
            }
            self.$http.get(configPath + 'home.json', [data]).then(successCallback, errorCallback)
        }
    }
}
</script>

<style src="../../../src/assets/styles/module/home/views.css">

</style>