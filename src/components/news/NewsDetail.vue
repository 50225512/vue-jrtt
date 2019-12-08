<template>
    <div id="main">
      <div class="app-container" id="news_article">
        <div class="weixin-share-portal"></div>
        <article class="article">
          <div style="height: auto;" class="hegto">
            <div class="article__header">
              <h1 class="article__title">{{news_details.article}}</h1>
              <div class="article__author">
                <div class="header-left">
                  <div class="avatar">
                    <img v-bind:src="news_details.avatar">
                  </div>
                  <div class="author-info">
                      <span class="author-name">{{news_details.autorname}}</span>
                      <span class="v-info v-0"></span>
                      <span class="author-original">{{news_details.is_original}}</span>
                  </div>
                </div>
                <div class="header-right">
                  <span class="new-style-test-article-publish-time">1个月前</span>
                  <span class="dot"></span>
                  <span class="new-style-test-article-comment">{{news_details.comment_count}}评论</span>
                  </div>
                </div>
            </div>
            <div class="article__content" id="article">
              <div v-if="news_details.has_video">
                <!-- <p><br></p> -->
                <div class="tt-video-box" v-bind:tt-video-id="news_details.video_id"
                v-bind:tt-poster="news_details.poster_url">
                  <video v-bind:src="decode_video_url" v-bind:poster="news_details.poster_url" 
                  controls="" width="640" height="320" preload="true" webkit-playsinline="webkit-playsinline" 
                  playsinline="playsinline" id="tt-video" class="tt-video">
                  </video>
                </div>
              </div>
              <div v-html="news_details.content">
                {{news_details.content}}
              </div>
            </div>
          </div>
        </article>
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
        news_details: {},
        encode_video_url: ""  ,
        decode_video_url: ""     
    };
  },
  created(){
    this.getNewsdetail();
  },
  mounted(){
    mui(".mui-scroll-wrapper").scroll({
        deceleration: 0.0005
    });
  },

  methods: {
    getNewsdetail() {
        // console.log(this.$route.query)
        // let fullpath = "/"+this.$route.fullPath;
        category = this.$route.query.category;
        tag_id = this.$route.query.tag_id;
      this.$http.get("news_details/", {params:{'category':category, "tag_id":tag_id}}).then(result => {
      //  console.log(result)
        this.news_details = result.body.news;
        this.encode_video_url = this.news_details.encode_video_url;
        console.log(this.encode_video_url);
        this.$http.jsonp(this.encode_video_url).then(result=>{
        this.decode_video_url = atob(result.body.data.video_list.video_1.backup_url_1);
        });
       
      });
    },
   

  },
};
</script>

<style lang="scss">
html, body, div, span, object, button, iframe, h1, h2, h3, h4, h5, h6, p, blockquote, a, code, em, img, q, small, strong, dd, dl, dt, li, ol, ul, fieldset, form, label, table, tbody, tr, th, td, input {
    margin: 0;
    padding: 0;
    border: 0;
}
html{
  font-size: 12px;
  background: #fff;
  font-family: 'PingFangSC','STHeiti','Microsoft YaHei','Helvetica','Arial',sans-serif;
  word-break: break-word;
  color: -internal-root-color;
}

:-webkit-any(article,aside,nav,section) h1 {
    margin-block-start: 0.83em;
    margin-block-end: 0.83em;
}

h1 {
    margin-inline-start: 0px;
    margin-inline-end: 0px;
}

div{
  margin: 0;
  padding: 0;
  border: 0;
  display: block;
  #news_article{
    padding-bottom: 70px;
    .article{
      padding-top: 8px;
      position: relative;
      overflow: hidden;
      display: block;
      .hegto{
        .article__header{
          position: relative;
          padding: 0 14px;
          .article__title{
            font-size: 24px;
            line-height: 34px;
            font-family: 'PingFangSC-Medium';
            font-weight: 500;
            color: #222222;
          }
          .article__author{
            position: relative;
            margin-top: 12px;
            display: flex;
            justify-content: space-between;
            .header-left{
              display: flex;
              align-items: center;
              .avatar{
                width: 24px;
                height: 24px;
                border-radius: 24px;
                margin: auto;
                display: inline-block;
                img{
                  width: 100%;
                  height: 100%;
                  border-radius: inherit;
                }
              }
              .author-info{
                margin-left: 6px;
                display: flex;
                align-items: center;
                .author-name{
                  font-size: 14px;
                  color: #222;
                  font-weight: 700;
                  margin-right: 6px;
                }
                .v-info{
                  background-image: url(//s1.pstatp.com/growth/mobile_detail/image/v-0.tl0z9x5L.png);
                  width: 14px;
                  height: 14px;
                  background-size: 100% 100%;
                  display: inline-block;
                  margin-right: 10px;
                  vertical-align: -2px;
                }
                .author-original{
                  position: relative;
                  text-align: center;
                  margin-right: 4px;
                  padding: 0 1px;
                  height: 14px;
                  line-height: 14px;
                  font-size: 10px;
                  box-sizing: border-box;
                  color: #505050;
                  vertical-align: 1px;
                  display: inline-block;
                  border-radius: 2px;
                }
              }
            }
            .header-right{
              align-self: center;
              color: #999999;
              font-size: 12px;
              .dot{
                  display: inline-block;
                  width: 2px;
                  height: 2px;
                  border-radius: 1px;
                  background-color: #999;
                  margin: 0 4px;
                  vertical-align: middle;
              }
            }
          }
        }
        .article__content{
          position: relative;
          overflow: hidden;
          clear: both;
          padding: 0 14px;
          padding-bottom: 24px;
          img{
            width: 100%;
            display: block;
            margin-right: auto;
            margin-left: auto;
            margin-top: 4px;
            transition: opacity 200ms ease;
          }
          p,li,span{
            word-wrap: break-word;
            color: #222;
            text-align: justify;
            list-style-position: inside;
            list-style-type: square;
            margin-top: 17px;
            font-size: 18px;
            line-height: 1.76;
            border: none;
            outline: none;
            max-height: 100%;
          }
          .tt-video-box{
            margin-top: 20px;
            min-height: initial;
            background: url(//s1.pstatp.com/growth/mobile_detail/image/xigua_loading.oPZoly1v.png) no-repeat center center;
            background-size: 50%;
            font-size: 0;
            margin-right: auto;
            margin-left: auto;
            width: 100%;
            height: 6.625rem;
            .tt-video{
              background-color: initial;
              width: 100%;
              height: 17.625rem;
              object-fit: contain;
            }
          }
        }
      }
    }
  }
}

</style>


