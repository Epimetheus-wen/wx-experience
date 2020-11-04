<script>
import md5 from "js-md5";
export default {
  onLaunch: function() {
    console.log("App Launch");
	wx.showShareMenu({
      withShareTicket: true,
	})
	
  },
  onShow: function() {
    console.log("App Show");
    wx.checkSession({
      success() {
        console.log("未过期");
        //session_key 未过期，并且在本生命周期一直有效
           uni.getStorage({
            key: "phone",
            success: function(res) {
                console.log(res.data);
            }
            });
      },
      fail() {
          let that=this
        wx.login({
          success: function(res) {
            if (res.code) {
              //发起网络请求
              console.log(res.code);
              uni.setStorage({
                key: "code",
                data: res.code,
                success: function() {
                  console.log("success");
                }
              });
              setTimeout(() => {
                console.log(res.code);
                let time = Math.round(new Date().getTime() / 1000).toString();
                let code = res.code;
                wx.request({
                  url:
                    // "https://test.callas2.caih.com/console/trialRequest.html?f=getKey",//测试
                    "https://api.caih.com/console/trialRequest.html?f=getKey",//生产
                  data: {
                    code: code
                  },
                  header: {
                    "content-type": "application/x-www-form-urlencoded",
                    sign: md5("DONGXINYITONG-ONLINE!" + time),
                    signtime: time
                  },
                  method: "POST",
                  success(resse) {
                    console.log(resse.data);
                    // uni.setStorage({
                    //   key: "sessionKey",
                    //   data: resse.data.data,
                    //   success: function() {
                    //     console.log("success");
                    //   }
                    // });
                  }
                });
              }, 1000);
            } else {
              console.log("获取用户登录态失败！" + res.errMsg);
            }
          }
        });
      }
    });
  },
  onHide: function() {
    console.log("App Hide");
  },
};
</script>

<style lang="scss">
/*每个页面公共css */
* {
  margin: 0;
  padding: 0;
}
.uni-popup {
  z-index: 99999999 !important;
}
.pop {
  .center {
    z-index: 99999999 !important;
  }
}
.right {
  z-index: 1 !important;
  s-identify {
    z-index: 1 !important;
  }
}
</style>
