<template>
  <div>
    <div class="listContainer">
      <swiper indicator-dots indicator-color="#888" indicator-active-color="#eee" autoplay=true>
        <swiper-item v-for="item in images" :key="item">
          <img :src="item.src" alt="" mode="aspectFill">
        </swiper-item>
      </swiper>
    </div>
    <div class="button_box">
      <div class="scan" @tap="goQrcodeDetail">扫码入场</div>
      <div class="down_userContainer" @tap="gotTimetable">
        <img class="down_userContainer_left" src="/static/tab/home.png" alt="">
        <span class="down_userContainer_text">课程表</span>
        <span class="down_userContainer_text_right">></span>
      </div>
      <div class="down_userContainer" @tap="goNotice">
        <img class="down_userContainer_left" src="/static/tab/home.png" alt="">
        <span class="down_userContainer_text">公告</span>
        <span class="down_userContainer_text_right">></span>
      </div>
      <div class="down_userContainer" @tap="goAboutus">
        <img class="down_userContainer_left" src="/static/tab/home.png" alt="">
        <span class="down_userContainer_text">关于我们</span>
        <span class="down_userContainer_text_right">></span>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        images: [  //数据
          { src: "cloud://liguangchao-u14ya.6c69-liguangchao-u14ya-1300277699/coach_photo/coach_00.jpg" },
          { src: "cloud://liguangchao-u14ya.6c69-liguangchao-u14ya-1300277699/coach_photo/coach_01.jpg" },
        ]
      }
    },
    methods: {
      goQrcodeDetail() {
        wx.navigateTo({
          url: '/pages/scanCode/main'
        })
      },
      gotTimetable() {
        wx.navigateTo({
          url: '/pages/timetable/main'
        })
      },
      goNotice() {
        wx.navigateTo({
          url: '/pages/notice/main'
        })
      },
      goAboutus(){
        wx.navigateTo({
          url: '/pages/aboutus/main'
        })
      }
    }
  }
</script>

<style>
  swiper {
    width: 96%;
    height: 450rpx;
    margin: 10rpx auto 0 auto
  }

  swiper img {
    width: 100%;
    height: 100%;
  }

  .button_box {
    width: 96%;
    height: 600rpx;
    margin: 20rpx auto 0 auto;
  }

  .scan {
    width: 60%;
    height: 70rpx;
    border: #333 1rpx solid;
    margin: 20rpx auto 0 auto;
    text-align: center;
    border-radius: 40rpx;
    line-height: 70rpx;
    color: #555
  }

  .down_userContainer {
    width: 90%;
    height: 90rpx;
    background-color: white;
    margin: 30rpx auto 0 auto;
    border-bottom: 1rpx #eee solid;
    display: flex;
    line-height: 80rpx;
  }

  .down_userContainer_left {
    margin-left: 20rpx;
    margin-top: 10rpx;
    width: 50rpx;
    height: 50rpx;
  }

  .down_userContainer_text {
    height: 50rpx;
    margin-left: 70rpx;
    font-size: 35rpx;
    width: 70%;
    letter-spacing: 4rpx;
    font-size: 28rpx;
  }

  .userContainer_text_right {
    height: 50rpx;
    font-size: 35rpx;
    float: right;
  }
</style>