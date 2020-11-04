<template>
	<view class="suspendButton">
		<button class="su_Button" type="primary" @tap="share(haibaoObj)">
			<image class="img" src="../static/privacy/fenxiang@1x.png" mode=""></image>
		</button>
		<button class="su_Button" type="primary" @tap="service">
			<image class="img" src="../static/privacy/kefu@1x.png" mode=""></image>
		</button>
		<button class="su_Button" type="primary" @tap="refresh(refreshUrl)">
			<image class="img" src="../static/privacy/shuaxin@1x.png" mode=""></image>
		</button>
		<uni-popup class="pop" ref="popup2" type="center" @change='handelChange'>
			<view class="pop_main">
				<view class="pop_main_center">
					<!-- <image class="img" :src="imgCode" mode=""></image> -->
					<canvas v-show="canva" id="code-canvas" canvas-id="codecanvas" :style="'width:'+(230*rpx)+'px;height:'+(244*rpx)+'px;'"></canvas>
				</view>
				<view class="pop_main_footer">
					<button class="pop_Button" open-type="share">
						<image class="img" src="../static/privacy/weixin@1x.png" mode=""></image><text>转发小程序</text>
					</button>
					<!-- <button class="pop_Button" type="primary" @tap="pengyouquan">
						<image class="img" src="../static/privacy/pengyouquan@1x.png" mode=""></image><text>转发至朋友圈</text>
					</button> -->
					<button class="pop_Button" type="primary" @tap="download">
						<image class="img" src="../static/privacy/download@1x.png" mode=""></image><text>保存海报</text>
					</button>
				</view>
			</view>
		</uni-popup>
	</view>
</template>

<script>
	import uniPopup from '@dcloudio/uni-ui/lib/uni-popup/uni-popup.vue'
	import md5 from "js-md5";
	export default {
		props: ['refreshUrl','haibaoObj'],
		data() {
			return {
				yccode: true,
                imgCode: "",
                imgBg:"",
                Phone: "",
                rpx:1,
                canva:false,
                haibaoUrl:"",
			};
		},
		components: {
			uniPopup
		},
		onReady() {
            console.log("haibaoObj",this.haibaoObj)
			let that = this
			uni.getStorage({
				key: "phone",
				success: function(res) {
					console.log(res.data);
					if (res.data && res.data != undefined && res.data != "undefined") {
                        that.Phone = res.data;
                        that.getImg(that.haibaoObj.id)
					}
				}
            });
            

		},
		methods: {
			getImg(src) {
                console.log(src)
				let that = this
				let time = Math.round(new Date().getTime() / 1000).toString();
				wx.request({
					url: that.$reUrl+"/console/trialRequest.html?f=getQrCode",
					data: {
						identifying: src
					},
					header: {
						"content-type": "application/x-www-form-urlencoded",
						sign: md5("DONGXINYITONG-ONLINE!" + time),
						signtime: time,
						phone: that.Phone
					},
					method: "POST",
					success(ress) {
						console.log(ress)
                        that.imgCode = "https:" + ress.data.data
                        that.setcodeImg()
					}
				})
			},
			setcodeImg() {
                let that=this
                wx.getSystemInfo({
                    success: function(res) {
                    that.rpx = res.windowWidth / 375;
                    }
                });
                wx.downloadFile({
                    url: that.imgCode, 
                    success (res) {
                        if (res.statusCode === 200) {
                            console.log(res)
                            that.imgCode=res.tempFilePath
                        }
                    }
                })
			},
			handelChange(e) {
                this.$emit("event", e.show);
                if(!e.show){
                    this.canva=false
                }
			},
			share(obj) {
				console.log("share")
                this.$refs.popup2.open()
                let that=this
                const codecanvas = wx.createCanvasContext("codecanvas", this);
                codecanvas.drawImage("../static/privacy/bg@1x.png", 0, 0, (230*that.rpx), (244*that.rpx))
                codecanvas.drawImage("../static/privacy/"+obj.icon, (30*that.rpx), (180*that.rpx), (25*that.rpx), (25*that.rpx))
                codecanvas.drawImage(that.imgCode, (50*that.rpx), (30*that.rpx), (125*that.rpx), (125*that.rpx))
                codecanvas.fillStyle="#242424"
                if(that.rpx<=1){
                    // codecanvas.font=(28*that.rpx)+'px SimHei'
                    console.log(that.rpx*14)
                    let num=Math.round(that.rpx*14)
                    codecanvas.font=num+'px SimHei'
                }else{
                    // codecanvas.font=(14*that.rpx)+'px SimHei'
                    console.log(that.rpx*14)
                    let num=Math.round(that.rpx*14)
                    codecanvas.font=num+'px SimHei'
                }
                // codecanvas.font='28px SimHei'
                codecanvas.fillText(obj.txt+"试用",(67*that.rpx),(194*that.rpx))
                
                codecanvas.fillStyle="#797979"
                console.log(that.rpx)
                // if(that.rpx>1){
                //     codecanvas.font=(26*that.rpx)+'px SimHei'
                // }else{
                //     codecanvas.font=(13*that.rpx)+'px SimHei'
                // }
                codecanvas.fillText("扫一扫，即可前往试用",(67*that.rpx),(214*that.rpx))
                    codecanvas.draw(
                        false,
                        setTimeout(function() {
                        wx.canvasToTempFilePath({
                            canvasId: "codecanvas",
                            success: function(res) {
                                console.log(res)
                                that.haibaoUrl = res.tempFilePath;
                            },
                            fail: function (e) {
                                console.log("AAAA", e)
                            }
                        },that);
                        }, 1000)
                    );

                setTimeout(()=>{
                    that.canva=true
                },500)

			},
			service() {
				console.log("service")
				uni.navigateTo({
					url: "/pages/outside/outside?id=kefu"
				});
			},
			refresh(src) {
				var that = this
				console.log(src)
				uni.redirectTo({
					url: '/pages/product/' + src,
				});
			},
			weixin() {
				console.log("weixin")
			},
			pengyouquan() {
				console.log("pengyouquan")
			},
			download() {
				console.log("download")
				let that = this
				wx.getSetting({
					success(res) {
						//未授权 先授权 然后保存
						if (!res.authSetting['scope.writePhotosAlbum']) {
							wx.authorize({
								scope: 'scope.writePhotosAlbum',
								success(re) {
									that.saveToBlum();
								}
							})
						} else {
							//已授 直接调用保存到相册方法
							that.saveToBlum();
						}
					}
				})


			},
			saveToBlum() {
				let imgUrl = this.haibaoUrl;
				console.log(imgUrl)
				wx.getImageInfo({
					src: imgUrl,
					success: function(ret) {
						var path = ret.path;
						console.log(ret)
						wx.saveImageToPhotosAlbum({
							filePath: path,
							success(result) {
								wx.showToast({
									title: '保存成功',
									icon: 'success'
								})
							},
						})
					},
					fail: function(ret) {
						console.log("fail====>", ret)
					}
				})
			}
		},

	}
</script>

<style lang="scss">
	.suspendButton {
		position: fixed;
		right: 10rpx;
		bottom: 24rpx;
		z-index: 2;

		.su_Button {
			width: 64rpx;
			height: 64rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			padding: 0;
			border-radius: 50%;
			background: #c4c4c4;
			border: none;
			margin-bottom: 10rpx;

			.img {
				display: block;
				width: 34rpx;
				height: 34rpx;
			}
		}

		.su_Button::after {
			border: none;
		}

		.pop {
			.pop_main {
				.pop_main_center {
					height: 488rpx;
					width: 460rpx;

					.img {
						width: 100%;
						width: 100%;
						display: block;
					}
				}

				.pop_main_footer {
					position: fixed;
					bottom: 0;
					left: 0;
					width: 100%;
					height: 200rpx;
					display: flex;
					justify-content: space-between;
					align-items: center;
					background: #fff;

					.pop_Button {
						display: flex;
						flex-direction: column;
						align-items: center;
						justify-content: center;
						padding: 0;
						background: #fff;

						&::after {
							border: none;
						}

						.img {
							width: 76rpx;
							height: 76rpx;
							display: block;
						}

						text {
							color: #242424;
							font-size: 25rpx;
						}
					}
				}
			}
		}
	}
</style>
