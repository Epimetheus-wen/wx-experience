<template>
  <view class="web-main">
    <web-view :webview-styles="webviewStyles" :src="id"></web-view>
  </view>
</template>
<style>
</style>
<script>
export default {
  data() {
    return {
      id: ""
    };
  },
  onLoad: function(option) {
    //option为object类型，会序列化上个页面传递的参数
    wx.showShareMenu({
      withShareTicket: true
    });
    console.log(option.id);
    if (option.id == "kefu") {
      this.id =
        "https://dxykf.caih.com/chat/h5/index.html?sysNum=3677f2094abc4fada9b6e657de8b2cfc";
    } else {
      this.id = this.$reUrl + "/web/#/" + option.id;
    }
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
  }
};
</script>