<template>
    <div>
        <!--轮播图-->
        <mt-swipe :auto="3000" :prevent="true">
            <mt-swipe-item v-for="item in lunbotuList" :key="item.url">
                <img :src="item.url" alt="">
            </mt-swipe-item>
         
        </mt-swipe>

        <!--九宫格到6宫格的改造-->
        <ul class="mui-table-view mui-grid-view mui-grid-9">
            <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
                <router-link to="/home/newslist">
                    <img src="../../images/menu1.png" alt="">
                    <div class="mui-media-body">新闻资讯</div>
                </router-link></li>
            <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3"><router-link to="/home/photolist">
                    <img src="../../images/menu2.png" alt="">
                    <div class="mui-media-body">图片分享</div></router-link></li>
            <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3"><router-link to="#">
                    <img src="../../images/menu3.png" alt="">
                    <div class="mui-media-body">商品购买</div></router-link></li>
            <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3"><router-link to="#">
                    <img src="../../images/menu4.png" alt="">
                    <div class="mui-media-body">留言反馈</div></router-link></li>
            <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3"><router-link to="#">
                    <img src="../../images/menu5.png" alt="">
                    <div class="mui-media-body">视频分享</div></router-link></li>
            <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3"><router-link to="#">
                    <img src="../../images/menu6.png" alt="">
                    <div class="mui-media-body">联系我们</div></router-link></li>
           
		</ul> 

    </div>
</template>

<script>
import { Toast } from 'mint-ui'
export default{
    data(){
        return {
            lunbotuList:[] //保存轮播图的数组
        }
    },
    created(){
        this.getLunbotu();
    },
    methods:{
        getLunbotu(){
            this.$http.get('https://www.apiopen.top/meituApi?page=3').then(result=>{
                
                if(result.body.code ===200){
                    // result.body.data.slice(0,4).forEach((item,index) => {
                    //     this.lunbotuList.push(item.url)
                    // })

                    
                    this.lunbotuList = result.body.data.slice(0,4);

                }else{
                    Toast('加载轮播图失败...')
                }
            })
        }
    }
}

</script>

<style lang="scss" scoped>
.mint-swipe{
    height: 200px;

    .mint-swipe-item{
        // &:nth-child(1){

        // }
        // &:nth-child(2){

        // }
        // &:nth-child(3){
            
        // }

        img{
            width:100%;
            height:100%;
        }
    }
}

.mui-grid-view.mui-grid-9{
    background-color: #fff;
    border: none;    
    
    img{
        width: 60px;
        height: 60px;
        
    }
    .mui-media-body{
        font-size:13px;
    }
}

.mui-grid-view.mui-grid-9 .mui-table-view-cell{
    border: 0;
}
    
</style>
