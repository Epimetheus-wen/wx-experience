<template>
  <view class="puzzle">
    <view
      class="verificationPuzzleContainer"
      :style="'width:'+(215*rpx)+'px;height:'+(38*rpx)+'px;'"
    >
      <view v-show="toum" class="view-z-container" :style="'width:'+(215*rpx)+'px;height:'+(120*rpx)+'px;top:'+-(120*rpx)+'px;'">
        <canvas
          class="canvas-shadow"
          :style="'width:'+(215*rpx)+'px;height:'+(120*rpx)+'px;z-index:10;'"
          disable-scroll="true"
          canvas-id="canvasShadow"
        ></canvas>
        <cover-image
          class="coimg"
          :style="'width:'+(215*rpx)+'px;height:'+(120*rpx)+'px;z-index:5;'"
          :src="oriSrc"
        ></cover-image>
        <canvas
          class="canvas-puzzle"
          :style="'width:'+(215*rpx)+'px;height:'+(120*rpx)+'px;z-index:10;transform:translateX('+translateX+'px);'"
          disable-scroll="true"
          canvas-id="canvasPuzzle"
        ></canvas>
      </view>

      <!-- 滑块 -->
      <view class="sliders-item" :style="'width:'+(215*rpx)+'px;height:'+(38*rpx)+'px;'">
        <text>{{ sliders.def }}</text>
        <view
          class="operationBgBox"
          :style="'height:'+(38*rpx)+'px;left:'+-(sliders.xRange)+'px;transform:' + sliders.animation"
        >
          <text>{{ sliders.succeedMsg }}</text>
          <view
            class="operationBox"
            :style="'width:'+(38*rpx)+'px;height:'+(38*rpx)+'px;'"
            @touchmove="touchmove"
            @touchend="touchend"
          >
            <img
              :style="'width:'+(38*rpx)+'px;height:'+(38*rpx)+'px;'"
              src="../static/privacy/huakiuai@1x.png"
              alt
            />
          </view>
        </view>
      </view>
    </view>
    <!-- <cgSlider @event="event"></cgSlider> -->
  </view>
</template>

<script>
export default {
  data() {
    return {
      oriSrc: "../static/privacy/puzz.png",
      imgPuzzle: "",
      imgShadow: "",
      translateX: 0,
      oriX: 0,
      x: 100,
      oldx: 0,
      isOk: false,
      size: {},
      rpx: 1,
      sliders: {
        def: "向右滑动完成拼图",
        xAxial: 0, //X轴的初始值
        x: 0, //触摸时X轴的值
        xRange: "", //滑块可移动的X轴范围
        animation: "translate3d(0, 0, 0)", //CSS动画的初始值
        succeedMsg: "", //验证成功提示信息的默认值
        pullStatus: true
      },
      toum:false
    };
  },
  components: {
  },
  onReady() {
    // this.width = this.properties.width;
    // this.height = this.properties.height;

    this.drawPic(100, 70, 5);
  },
  updated(){
     
    console.log("this.sliders.xRange",this.sliders.xRange)
  },
  created() {
    this.sliders.def = "向右滑动完成拼图";
    this.sliders.xAxial = 0;
    this.sliders.x = 0;
    this.sliders.xRange = 177*this.rpx; 
    this.sliders.animation = "translate3d(0, 0, 0)";
    this.sliders.succeedMsg = "";
    this.sliders.pullStatus = true;
  },
  methods: {
    touchmove(e) {
      //   如验证成功后仍允许滑动，则执行下面代码块（初始值默认为允许）
      // this.$emit("event", this.x);
      // let tX=this.translateX
      this.toum=true
      if (this.sliders.pullStatus) {
        console.log(this.sliders.x);
        //设置X轴的始点
        this.sliders.x = e.changedTouches[0].clientX - (19 * this.rpx)-(120 * this.rpx);
        this.translateX = -this.x + this.sliders.x;
        //如果触摸时X轴的坐标大于可移动距离则设置元素X轴的坐标等于可移动距离的最大值，否则元素X轴的坐标等于等于当前触摸X轴的坐标
        if (this.sliders.x >= this.sliders.xRange - 19 * this.rpx) {
          this.sliders.xAxial = this.sliders.xRange;
          this.translateX = this.sliders.xRange;
        } else {
          this.sliders.xAxial = this.sliders.x;
        }
        //如果触摸时X轴的坐标小于设定的始点，则将元素X轴的坐标设置为0
        if (this.sliders.x < 19 * this.rpx) {
          this.sliders.xAxial = 0;
          this.translateX = -this.x;
        }
        //根据获取到的X轴坐标进行动画演示
        this.sliders.animation =
          "translate3d(" + this.sliders.xAxial + "px, 0, 0)";
      }
    },

    touchend() {
      var data = {};
      //如果触摸的X轴坐标大于等于设置的可移动范围，则验证成功
      if (this.sliders.x >= this.x - 5 && this.sliders.x <= this.x + 5) {
        //X轴坐标等于可移动范围的最大值
        this.sliders.xAxial = this.sliders.xRange;
        //验证成功提示语
        this.sliders.succeedMsg = "验证成功";
        //设置返回值
        data.msg = true;
        //验证成功禁止滑动
        this.sliders.pullStatus = false;
        this.$emit('event',data)
      } else {
        //X轴坐标归0
        this.sliders.xAxial = 0;
        this.translateX = -this.x;
        //清空成功提示
        this.sliders.succeedMsg = "";
        //设置返回值
        data.msg = false;
      }
      //根据获取的X轴坐标进行动画演示
      this.sliders.animation =
        "translate3d(" + this.sliders.xAxial + "px, 0, 0)";
      // 返回验证结果
      //   this.$emit("event", data);
      setTimeout(()=>{
          this.toum=false
      },500)
    },
    drawPic(x, y, r) {
      var that = this;
      var ctxShadow;
      var ctxPuzzle;
      //获取屏幕宽度，获取自适应单位
      wx.getSystemInfo({
        success: function(res) {
          that.rpx = res.windowWidth / 375;
          that.sliders.xRange=177*that.rpx
        }
      });
      console.log(that.rpx);
      that.translateX = -x;
      that.oriX = -x;
      ctxPuzzle = wx.createCanvasContext("canvasPuzzle", this);
      ctxShadow = wx.createCanvasContext("canvasShadow", this);
      that.clip(ctxPuzzle, x, y, r * that.rpx);
      that.clip(ctxShadow, x, y, r * that.rpx);
      wx.getImageInfo({
        src: "../static/privacy/puzz.png",
        success: function(res) {
          console.log("res", res);
          ctxShadow.drawImage(res.path, 0, 0, 215 * that.rpx, 120 * that.rpx);
        }
      });

      ctxShadow.drawImage(that.oriSrc, 0, 0, 215 * that.rpx, 120 * that.rpx);
      ctxShadow.setFillStyle("black");
      ctxShadow.setGlobalAlpha(0.5);
      ctxShadow.fillRect(0, 0, 250, 150);
      ctxShadow.draw(
        false,
        setTimeout(function() {
          wx.canvasToTempFilePath({
            canvasId: "canvasShadow",
            success: function(res) {
                console.log("res")
            //   that.data.tmpPath = res.tempFilePath;
            },
              fail: function (e) {
                console.log("AAAA", e)
              }
          },that);
        }, 100)
      );

      ctxPuzzle.drawImage(that.oriSrc, 0, 0, 215 * that.rpx, 120 * that.rpx);
      wx.getImageInfo({
        src: "../static/privacy/puzz.png",
        success: function(res) {
          console.log("res", res);

          ctxPuzzle.drawImage(res.path, 0, 0, 215 * that.rpx, 120 * that.rpx);
        }
      });
      ctxPuzzle.setFillStyle("black");
      ctxPuzzle.setGlobalAlpha(0);
      ctxPuzzle.fillRect(0, 0, 215 * that.rpx, 120 * that.rpx);
      ctxPuzzle.draw(
        false,
        setTimeout(function() {
          wx.canvasToTempFilePath({
            canvasId: "canvasPuzzle",
            success: function(res) {
              that.data.tmpPath = res.tempFilePath;
              console.log(res)
            }
          });
        }, 1000)
      );
    },
    clip(ctx, x, y, r) {
      ctx.save();
      //开始一个新的绘制路径
      ctx.beginPath();
      //设置路径起点坐标
      ctx.moveTo(x, y);
      ctx.arcTo(x, y - r, x + r, y - r, r);
      ctx.lineTo(x + 2 * r, y - r);
      ctx.arcTo(x + 2 * r, y - 2 * r, x + 3 * r, y - 2 * r, r);
      ctx.arcTo(x + 4 * r, y - 2 * r, x + 4 * r, y - r, r);
      ctx.lineTo(x + 5 * r, y - r);
      ctx.arcTo(x + 6 * r, y - r, x + 6 * r, y, r);
      ctx.lineTo(x + 6 * r, y + r);
      ctx.arcTo(x + 7 * r, y + r, x + 7 * r, y + 2 * r, r);
      ctx.arcTo(x + 7 * r, y + 3 * r, x + 6 * r, y + 3 * r, r);
      ctx.lineTo(x + 6 * r, y + 4 * r);
      ctx.arcTo(x + 6 * r, y + 5 * r, x + 5 * r, y + 5 * r, r);
      ctx.lineTo(x + 4 * r, y + 5 * r);
      ctx.arcTo(x + 4 * r, y + 4 * r, x + 3 * r, y + 4 * r, r);
      ctx.arcTo(x + 2 * r, y + 4 * r, x + 2 * r, y + 5 * r, r);
      ctx.lineTo(x + r, y + 5 * r);
      ctx.arcTo(x, y + 5 * r, x, y + 4 * r, r);
      ctx.lineTo(x, y + 3 * r);
      ctx.arcTo(x + r, y + 3 * r, x + r, y + 2 * r, r);
      ctx.arcTo(x + r, y + r, x, y + r, r);
      ctx.lineTo(x, y);
      //先关闭绘制路径。注意，此时将会使用直线连接当前端点和起始端点。
      ctx.closePath();
      ctx.clip();
      ctx.stroke(); //画线轮廓
    }
  }
};
</script>

<style lang="scss">
.verificationPuzzleContainer{
    position: relative;
}
.view-z-container {
  position: absolute;
  overflow: hidden;

  left: 0;
  .canvas-shadow {
    //  width:430rpx;
    //  height:240rpx;
    z-index: 11;
    position: absolute;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    margin: auto;
  }
  .canvas-puzzle {
    position: absolute;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    margin: auto;
  }
  .coimg {
    position: absolute;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    margin: auto;
  }
}
.sliders-item {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 26rpx;
  overflow: hidden;
  background: #f5f9ff;
  border-radius: 10rpx;
  border: 1px solid #e9ecf2;
  .operationBgBox {
    width: 100%;
    background-color: #7bbb55;
    overflow: hidden;
    position: absolute;
    top: 0;
    color: #fff;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .operationBox {
    background-color: #ebebeb;
    display: flex;
    align-items: center;
    justify-content: center;
    position: absolute;
    right: 0;
    top: 0;
  }
}
</style>
