<template>
  <view class="home">
    <view class="status_bar" :style="'height: calc('+((staHeight*2)+80)+'rpx);'">
      <view class="top_view" :style="'height:'+staHeight+'px;'"></view>
      <view class="top titles">
        <view class="titleText">易通信体验中心</view>
      </view>
    </view>
    <view class="geduan" :style="'height: calc('+((staHeight*2)+80)+'rpx);'"></view>

    <view class="home_main">
      <!-- 轮播图 -->
      <view class="banner">
        <view class="uni-padding-wrap">
          <view class="page-section swiper">
            <view class="page-section-spacing">
              <swiper
                class="swiper"
                :indicator-dots="indicatorDots"
                indicator-active-color="#076bff"
                :autoplay="autoplay"
                :interval="interval"
                :duration="duration"
                :circular="true"
                @change="change"
              >
                <swiper-item v-for="(item ,index) in list" :key="index">
                  <view class="swiper-item">
                    <img @tap="wailian(item.to)" :src="item.imgUrl" alt />
                  </view>
                </swiper-item>
              </swiper>
              <swiper-dot class="dot" :current="current" :list="list"></swiper-dot>
            </view>
          </view>
        </view>
      </view>

      <!-- 热门产品体验 -->
      <view class="popular_product">
        <view class="product_title">热门产品体验</view>
        <view class="product_center">
          <view class="product_center_item border_btm border_rgt" @tap="product('privacy')">
            <img mode="aspectFit" class="img" src="../../static/home/yinsihao@1x.png" alt />
            <view class="text">隐私号</view>
          </view>
          <view class="product_center_item border_btm" @tap="product('message')">
            <img mode="aspectFit" class="img" src="../../static/home/duanxinfuwu@1x.png" alt />
            <view class="text">短信服务</view>
          </view>
          <view class="product_center_item border_rgt" @tap="product('voice')">
            <img mode="aspectFit" class="img" src="../../static/home/yuyintongzhi@1x.png" alt />
            <view class="text">语音通知</view>
          </view>
          <view class="product_center_item">
            <!-- <img mode="aspectFit" class="img" src="../../static/home/yinsihao@1x.png" alt /> -->
            <!-- <view class="text"></view> -->
          </view>
        </view>
      </view>

      <!--号码检测体验 -->
      <view class="popular_product">
        <view class="product_title">号码检测体验</view>
        <view class="product_center">
          <view class="product_center_item border_btm border_rgt" @tap="product('vacantNumber')">
            <img mode="aspectFit" class="img" src="../../static/home/konghaojiance@1x.png" alt />
            <view class="text">空号检测</view>
          </view>
          <view class="product_center_item border_btm" @tap="product('twice')">
            <img mode="aspectFit" class="img" src="../../static/home/ercifanghaojiance@1x.png" alt />
            <view class="text">二次放号检测</view>
          </view>
          <view class="product_center_item border_rgt" @tap="product('networkTime')">
            <img
              mode="aspectFit"
              class="img"
              src="../../static/home/zaiwangshichangchaxun@1x.png"
              alt
            />
            <view class="text">在网时长查询</view>
          </view>
          <view class="product_center_item" @tap="product('realTime')">
            <img
              mode="aspectFit"
              class="img"
              src="../../static/home/haomashishizhuangtaichaxun@1x.png"
              alt
            />
            <view class="text">号码实时状态检测</view>
          </view>
        </view>
      </view>

      <!-- 身份核验体验 -->
      <view class="popular_product">
        <view class="product_title">身份核验体验</view>
        <view class="product_center">
          <view class="product_center_item border_btm border_rgt" @tap="product('idCard')">
            <img
              mode="aspectFit"
              class="img"
              src="../../static/home/shenfenzhengshimingrenzheng@1x.png"
              alt
            />
            <view class="text">身份证实名认证</view>
          </view>
          <view class="product_center_item border_btm" @tap="product('phoneAuth')">
            <img
              mode="aspectFit"
              class="img"
              src="../../static/home/shoujihaoshimingrenzheng@1x.png"
              alt
            />
            <view class="text">手机号实名认证</view>
          </view>
          <view class="product_center_item border_rgt" @tap="product('bankCardThree')">
            <img mode="aspectFit" class="img" src="../../static/home/yinhangkasanyaosu@1x.png" alt />
            <view class="text">银行卡三要素核验</view>
          </view>
          <view class="product_center_item" @tap="product('bankCardFour')">
            <img mode="aspectFit" class="img" src="../../static/home/yinhangkasiyaosu@1x.png" alt />
            <view class="text">银行卡四要素核验</view>
          </view>
        </view>
      </view>
    </view>
    <!-- <Puzzle v-if="typeId"></Puzzle> -->
    <button
      v-if="getPhone"
      class="PhoneNumber"
      open-type="getPhoneNumber"
      @getphonenumber="onGetPhoneNumber"
    ></button>
  </view>
</template>


<script>
import swiperDot from "../../components/seiperDot.vue";
// import Puzzle from "../../components/puzzle.vue";
import md5 from "js-md5";
export default {
  data() {
    return {
      list: [
        {
          name: "swiperSlide1",
          imgUrl: require("../../static/home/banner.png"),
          title: "注册享免费体验",
          text: "新用户立即注册可享短信免费体验！",
          to: "register"
        },
        {
          name: "swiperSlide2",
          imgUrl: require("../../static/home/banner.png"),
          title: "语音通知+挂机短信",
          text: "最大效率触达客户，确保用户知晓信息",
          to: "register"
        }
      ],
      current: 0,
      indicatorDots: false,
      autoplay: true,
      interval: 5000,
      duration: 500,
      getPhone: true,
      typeId: false,
      staHeight: ""
    };
  },
  components: {
    swiperDot
    // Puzzle
  },
  onShareAppMessage(res) {
    console.log("转发");
    return {
      title: "汇聚丰富场景的云通信产品，中国东信邀您免费试用!",
      path: "/pages/index/index",
      imageUrl: "../../static/home/share.png",
      success: function(ress) {
        console.log(ress);
      }
    };
  },
  onShow() {
    this.typeId = true;
    console.log(this.typeId);
  },
  onHide() {
    this.typeId = false;
    console.log(this.typeId);
  },
  onReady() {
    let that = this;
    this.$authorization();
    console.log("xxx");
    wx.getSystemInfo({
      success: e => {
        console.log("状态栏高度", e.statusBarHeight);
        that.staHeight = e.statusBarHeight;
      }
    });
    // this.typeId=true

    // wx.login({
    //     success: function(res) {
    //       if (res.code) {
    //         //发起网络请求
    //         console.log(res.code);
    //         that.codeLogin=res.code
    //       } else {
    //         console.log("获取用户登录态失败！" + res.errMsg);
    //       }
    //     }
    //   });

    uni.getStorage({
      key: "phone",
      success: function(res) {
        console.log(res.data);
        if (res.data && res.data != undefined && res.data != "undefined") {
          that.getPhone = false;
        }
      }
    });
  },
  methods: {
    wailian(src) {
      console.log(src);
      uni.navigateTo({
        url: "/pages/outside/outside?id=" + src
      });
    },
    change(e) {
      this.current = e.detail.current;
    },
    // login(e) {
    //   wx.login({
    //     success: function(res) {
    //       if (res.code) {
    //         //发起网络请求
    //         console.log(res.code);
    //       } else {
    //         console.log("获取用户登录态失败！" + res.errMsg);
    //       }
    //     }
    //   });
    // },
    onGetPhoneNumber(e) {
      console.log("onGetPhoneNumber", e);
      console.log(e.detail.errMsg);
      console.log(e.detail.iv);
      console.log(e.detail.encryptedData);
      let time = Math.round(new Date().getTime() / 1000).toString();
      let that = this;
      uni.getStorage({
        key: "code",
        success: function(res) {
          console.log(res.data);
          wx.request({
            url: that.$reUrl + "/console/trialRequest.html?f=getUserInfo",
            data: {
              encryptedData: e.detail.encryptedData,
              code: res.data,
              iv: e.detail.iv
            },
            header: {
              "content-type": "application/x-www-form-urlencoded",
              sign: md5("DONGXINYITONG-ONLINE!" + time),
              signtime: time
            },
            method: "POST",
            success(ress) {
              console.log(ress.data);
              if (ress.data.code == 0) {
                console.log(ress.data.data.phoneNumber);
                let phone = ress.data.data.phoneNumber;
                uni.setStorage({
                  key: "phone",
                  data: ress.data.data.phoneNumber,
                  success: function() {
                    console.log("success");
                    that.getPhone = false;
                  }
                });
                that.authorization(phone);
              } else {
                // wx.login({
                //   success: function(res) {
                //     if (res.code) {
                //       //发起网络请求
                //       console.log(res.code);
                //       uni.setStorage({
                //         key: "code",
                //         data: res.code,
                //         success: function() {
                //           console.log("success");
                //         }
                //       });
                //     } else {
                //       console.log("获取用户登录态失败！" + res.errMsg);
                //     }
                //   }
                // });
              }
            }
          });
        }
      });
    },
    product(src) {
      console.log(src);
      uni.navigateTo({
        url: "/pages/product/" + src
      });
    },
    authorization(mobile) {
      let time = Math.round(new Date().getTime() / 1000).toString();
      let that = this;
      wx.request({
        url: that.$reUrl + "/console/trialRequest.html?f=authorization",
        data: {
          nickname: mobile
        },
        header: {
          "content-type": "application/x-www-form-urlencoded",
          sign: md5("DONGXINYITONG-ONLINE!" + time),
          signtime: time,
          phone: mobile
        },
        method: "POST",
        success(ress) {
          console.log("授权");
          console.log(ress.data);
        }
      });
    }
  }
};
</script>

<style lang="scss">
.home {
  .status_bar {
    height: calc(var(--status-bar-height) + 80rpx);
    width: 100%;
    position: fixed;
    top: 0;
    background: #fff;
    z-index: 10;
  }

  .top_view {
    height: var(--status-bar-height);
    width: 100%;
    color: #000000;
  }

  .geduan {
    height: calc(var(--status-bar-height) + 80rpx);
  }

  .titles {
    height: 80rpx;

    .titleText {
      height: 80rpx;
      color: #000000;
      display: flex;
      align-items: center;
      font-size: 34rpx;
      margin-left: 42rpx;
      font-weight: 600;
    }
  }

  //   轮播图
  .banner {
    margin-top: 50rpx;

    .swiper {
      height: 260rpx;

      swiper-item {
        display: flex;
        justify-content: center;
        align-items: center;
      }

      .swiper-item {
        width: 690rpx;
        height: 260rpx;

        img {
          width: 100%;
          height: 100%;
          display: block;
        }
      }
    }

    .page-section-spacing {
      position: relative;

      .dot {
        position: absolute;
        bottom: 20rpx;
      }
    }
  }

  .home_main {
    padding-bottom: 32rpx;

    //   产品体验
    .popular_product {
      margin-top: 69rpx;
      padding: 0 30rpx;

      .product_title {
        font-size: 30rpx;
        font-weight: 600;
        margin-left: 8rpx;
      }

      .product_center {
        display: flex;
        flex-wrap: wrap;
        border-radius: 7rpx;
        border: 1rpx solid #e9edf3;
        margin-top: 23rpx;

        .product_center_item {
          height: 127rpx;
          width: 338rpx;
          display: flex;
          // justify-content: center;
          align-items: center;

          .img {
            width: 47rpx;
            margin-left: 20rpx;
            margin-right: 18rpx;
            display: block;
            height: 100%;
          }

          .text {
            font-size: 26rpx;
          }
        }

        .border_btm {
          border-bottom: 1rpx solid #e9edf3;
        }

        .border_rgt {
          border-right: 1rpx solid #e9edf3;
        }
      }
    }
  }
  .PhoneNumber {
    opacity: 0;
    height: 100vh;
    width: 100vw;
    position: fixed;
    top: 0;
    left: 0;
  }
}
</style>
