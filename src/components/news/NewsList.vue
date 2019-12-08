<template>
  <div>
      <!-- 顶部滑动条区域 --> 
      <div id="slider" class="mui-slider">
        <div id="sliderSegmentedControl" class="mui-scroll-wrapper mui-slider-indicator mui-segmented-control mui-segmented-control-inverted">
          <div class="mui-scroll">
            <a :class="['mui-control-item', item == '推荐' ? 'mui-active' : '']" v-for="item in cates" :key="item" @tap="getNewsList(item)">
              {{ item }}
            </a>
          </div>
        </div>
      </div>

    <div>
      <content class="feed-list-container">
        <div class="list_content" v-for="item in news_detail" :key="item.abstract">
          <section class="has_action" v-if="!item.middle_mode">
            <router-link :to="{path:'/news_details/', query:{category:item.category, tag_id:item.tag_id}}">
              <div class="item_detail">
                <h3 class="dotdot line3">{{ item.title }}</h3>
                <div class="list_image clearfix" v-if="item.image_list">
                  <ul v-for="i in item.image_list" :key="i">
                    <li class="list_img_holder">
                      <img v-lazy="i">
                    </li>
                  </ul>
                </div>
                <div class="item_info clearfix">
                  <div>
                    <span class="stick_label space" v-if="item.label">{{ item.label }}</span>
                    <span class="src space">{{ item.media_name }}</span>
                    <span class="cmt space">评论 3732</span>
                    <span class="time">14分钟前</span>
                  </div>
                </div>
              </div>
              ::after
            </router-link>
          </section>
          <section class="has_action middle_model" v-if="item.middle_mode">
            <router-link :to="{path:'/news_details/', query:{category:item.category, tag_id:item.tag_id}}">
              <div class="item_detail desc">
                <h3 class="dotdot line3 image-margin-right">{{ item.title }}</h3>
                <div class="item_info">
                  <div>
                    <span class="hot_label space">{{ item.hot}}</span>
                    <span class="src space">{{ item.media_name }}</span>
                    <span class="cmt space">评论 85</span>
                    <span class="time">14分钟前</span>
                  </div>
                </div>
              </div>
              <div class="list_img_holder">
                <img v-lazy="item.image_url">
              </div>
              ::after
            </router-link>
          </section>
        </div>
      </content>
    </div>
  </div>
</template>

<script>
// 1. 导入 mui 的js文件
import mui from "../../lib/mui/js/mui.min.js";

import { Toast } from "mint-ui";
export default {
  data() {
    return {
        cates: [],
        newslist: [], //新闻列表
        news_detail: []
    };
  },
  created(){
      this.getAllCategory();
      // this.getNewsList()
  },
  mounted(){
    mui(".mui-scroll-wrapper").scroll({
        deceleration: 0.0005
    });
  },

  methods: {
    getAllCategory() {
      this.$http.get("getCategory/").then(result => {
        console.log(result.status);
        if (result.status === 200) {
          // 手动拼接出一个最完整的 分类列表
          this.cates = result.body["category"];
          for(i=0;i<this.cates.length;i++){
            if(this.cates[i]=="推荐"){  
              this.cates.splice(i, 1);            
              this.cates.splice(0, 0, "推荐");
            }
          }
          console.log(result.body["category"])
          this.getNewsList("推荐");
        }
    });
    },
    getNewsList(Channel) {
      console.log("调用了"+Channel);
      switch(Channel){
        case "推荐":
          Channel = "__all__";
          break;
        case "视频":
          Channel = "video";
          break;
        case "社会":
          Channel = "news_society";
          break;
        case "军事":
          Channel = "news_military";
          break;
        case "娱乐":
          Channel = "news_entertainment";
          break;
        case "财经":
          Channel = "news_finance";
          break;
        case "科技":
          Channel = "news_tech";
          break;
        case "热门":
          Channel = "news_hot";
          break;
        case "运动":
          Channel = "news_sports";
          break;
        default:
          Channel = "__all__"
      }

      console.log("调用了"+Channel);
      this.$http.get("news_detail/", {params:{'Channel':Channel}}).then(result => {
         if (result.status === 200) {
          // 手动拼接出一个最完整的 分类列表
          // this.cates = result.body["category"];
          // this.news_detail = 
          console.log(result.data.news_details);
          this.news_detail = result.data.news_details;
          // console.log(result.body["category"])
        }
      });
    },
  },
};
</script>

<style lang="scss" scoped>
* {
  touch-action: pan-y;
}

div{
  line-height: 0px;
}

#slider{
    top: 40px;
    width: 100%;
    height: 37px;
    left: 0;
    z-index: 1000;
    position: fixed;
    background: #f4f5f6;
}

.clearfix::before,
.clearfix::after{
    content: "";
    display: block;
    visibility: hidden;
    height: 0;
    line-height: 0;
    clear: both;
}

.mui-segmented-control.mui-segmented-control-inverted .mui-control-item.mui-active {
    color: #D43D3D;
    border-bottom: 0;
    background: 0 0;
}

content.feed-list-container{
  font-family: 'STHeiti', 'Microsoft YaHei', 'Helvetica', 'Arial', sans-serif;
  -webkit-text-size-adjust: none;
  word-break: break-word;
  position: relative;
  display: block;
  top: 37px;
  .list_content{
    -webkit-margin-after-collapse: separate;
    -webkit-margin-before-collapse: discard;
    section{
      margin: 0px 15px;
      position: relative;
      display: block;
      -webkit-transition: all 1s ease-in-out;
      border-bottom: 1px solid rgba(221, 221, 221, 0.6);
      a{
        min-height: 42px;
        padding: 16px 0px;
        display: block;
        position: relative;
        font-size: 0px;
        text-decoration: none;
        -webkit-tap-highlight-color: rgba(0, 0, 0, 0.1);
        .desc{
          display: inline-block;
          width: 67%;
          vertical-align: middle;
          .image-margin-right{
            margin-right: 12px;
          }
        }
        .list_img_holder{
          overflow: hidden;
          width: 33%;
          display: inline-block;
          vertical-align: middle;
          height: 4.90625rem;
          position: relative;
          background: url(//s3b.pstatp.com/growth/mobile_list/image/toutiaoicon_loading_textpage@3x_f7c130ce.png) #efefef no-repeat center center;
          background-size: 50%;
          img{
              border: none;
              display: block;
              width: 100%;
              -webkit-transition: opacity 300ms ease;
              pointer-events: none;
              -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
              text-decoration: none;
          }
        }
        h3{
            color: #222;
            line-height: 21px;
            font-size: 17px;
            -webkit-line-clamp: 3;
            overflow: hidden;
            text-overflow: ellipsis;
            display: -webkit-box;
            -webkit-box-orient: vertical;
            font-weight: normal;


            margin-block-start: 1em;
            margin-block-end: 1em;
            margin-inline-start: 0px;
            margin-inline-end: 0px;
            max-height: 100%;
            margin: 0;
            padding: 0;
            border: 0;
        }
          .list_image{

            margin-top: 6px;
            ul{
              display: block;
              margin: 0;
              padding: 0;
              list-style-type: none;
              font-size: 0;
              text-align: center;
              border:0;

              li{
                display: inline-block;
                overflow: hidden;
                width: 33.3%;
                box-sizing: border-box;
              }

              li:first-child{
                padding-right: 4px;
                float: left;
              }

              .list_img_holder{
                height: 4.96875rem;
                position: relative;
                background: url(//s3b.pstatp.com/growth/mobile_list/image/toutiaoicon_loading_textpage@3x_f7c130ce.png) #efefef no-repeat center center;
                background-size: 50%;
                .img{
                  border: none;
                  display: block;
                  width: 100%;
                  -webkit-transition: opacity 300ms ease;
                  pointer-events: none;
                  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
                  text-decoration: none;
                }
              }
            }
          }
        .item_info {
          margin-top: 6px;
          color: #999;
          overflow: hidden;
          font-size: 0;
          .stick_label{
            border-radius: 2px;
            width: 25px;
            line-height: 12px;
            font-size: 9px;
            color: #f85959;
            border-color: rgba(248, 89, 89, 0.5);
            display: inline-block;
          }
          .hot_label{
            border-radius: 2px;
            width: 12px;
            line-height: 12px;
            font-size: 9px;
            color: #f85959;
          }
          .space{
            margin-right: 5px;
          }
          span{
            line-height: 12px;
            vertical-align: middle;
            font-size: 10px;
          }
        }
      }
    }
  }



}


</style>


