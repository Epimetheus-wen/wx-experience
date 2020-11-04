<template>
	<view class="privacy">
		<Header :titleTop="titleTop"></Header>
		<view class="privacy_main">
			<view class="banner">
				<image class="img" src="../../static/privacy/banner@1x.jpg" mode=""></image>
			</view>
			<view class="privacy_center">
				<view class="privacy_header">
					<view class="title">
						隐私号-AXB模式
					</view>
					<view class="xq_btn" @tap="wailian('product/privacy')">
						<text>产品详情</text>
						<image class="img" src="../../static/privacy/sanjiaoxing@1x.png" mode=""></image>
					</view>
				</view>
				<view v-if="typeid" class="privacy_from">
					<view class="top">
						<button @tap="open" class="yanshi">
							<image class="img" src="../../static/privacy/play@1x.png" mode=""></image><text>演示说明</text>
						</button>
						<uni-popup class="pop" ref="popup1" type="center" @change='handelChange'>
							<view class="txt">
								1.输入A号码（模拟商家平台）；<br />
								2.输入B号码（模拟被叫用户）；<br />
								3.完成滑块拼图后，点击立即试用，即可在5s内生成隐私号X，您可以拨打该号码模拟使用场景；<br />
								4.每个微信账号可拥有{{totalSum}}次免费试用的机会；<br />
								5.特别注意：请您勿将生成的X号码，用于营销、注册等行为，若因此产生了风险、投诉等后果，我方平台不予承担。
							</view>
						</uni-popup>
					</view>
					<view class="privacy_input">
						<view class="left">
							<image class="img" src="../../static/privacy/a_number@1x.png" mode=""></image>
							<text>A号码</text>
						</view>
						<input v-model="mobileA" class="uni-input srinput" placeholder="输入手机号码A" />
					</view>
					<view class="privacy_input">
						<view class="left">
							<image class="img" src="../../static/privacy/b_number@1x.png" mode=""></image>
							<text>B号码</text>
						</view>
						<input v-model="mobileB" class="uni-input srinput" placeholder="输入手机号码B" />
					</view>
                    <view class="privacy_input privacy_puzz">
						<view class="left">
							<image class="img" src="../../static/privacy/safe@1x.png" mode=""></image>
							<text>安全验证</text>
						</view>
                        <input v-model="imgCode" style="width: 216rpx;" class="uni-input srinput" placeholder="请输入验证码" />
                        <view v-show="popzindex" @tap="refreshCode" class="right" style="width:160rpx;height:64rpx;overflow: hidden;margin-bottom: 10rpx;margin-left: 50rpx;"><SIdentify style="width:160rpx;height:64rpx" :identifyCode="identifyCode"></SIdentify></view>
                        <!-- <view class="puz"><Puzzle @event='event'></Puzzle></view> -->
					</view>
				</view>
				<view v-if="typeid" class="privacy_footer">
					<view class="hint">
						<view class="title">
							<image class="img" src="../../static/privacy/xxx@1x.png" mode=""></image><text>隐私承诺</text>
						</view>
						<view class="txt">
							中国东信绝不会将您的手机号泄露给任何人，您所录入的信息只会用于该产品试用，可放心输入。
						</view>
					</view>
                    <button class="generate" type="primary" :loading='loading' :disabled='loading' @tap="generate"  v-text="loading?' 正在发送中':'生成隐私号'">生成隐私号</button>
					<view class="num">剩余可试用：<text>{{count}}</text>次</view>
                    <suspendButton :haibaoObj='haibaoObj' @event='event' :refreshUrl="'privacy'"></suspendButton>
				</view>
                <view v-if="!typeid" class="popItem">
                    <view class="popItem_top">
                        <view class="popItem_top_phone"><image class="img" src="../../static/privacy/a_number@1x.png" mode=""></image><text>{{newMobileA}}</text></view>
                        <text class="txt">&</text>
                        <view class="popItem_top_phone"><image class="img" src="../../static/privacy/b_number@1x.png" mode=""></image> <text>{{newMobileB}}</text></view>
                    </view>
                    <view class="popItem_t1">已成功绑定至隐私号X号码</view>
                    <view class="popItem_img"><image class="img" src="../../static/privacy/success@1x.png" mode=""></image></view>
                    <view class="popItem_t2" @tap='phoneCall(dstVirtualNum)'>{{dstVirtualNum}}</view>
                    <view class="popItem_t3">(您可在5分钟内拨打此号码)</view>
                </view>
                <view v-if="!typeid" class="privacy_footer">
                    <view class="butGather">
                        <button class="generate generate1" type="primary" @tap="back">不再试用</button>
                        <button class="generate" type="primary" @tap="backgen">再次试用</button>
                    </view>
					
					<view class="num">剩余可试用：<text>{{count}}</text>次</view>
				</view>
				<!-- <web-view :webview-styles="webviewStyles" src="http://api.caih.com/web/#/home"></web-view> -->
			</view>
            
		</view>
	</view>
</template>

<script>
    import suspendButton from "../../components/suspendButton.vue"
	import Header from "../../components/header.vue";
    import uniPopup from '@dcloudio/uni-ui/lib/uni-popup/uni-popup.vue'
    // import Puzzle from "../../components/puzzle.vue";
    import SIdentify from "../../components/identify";
    import md5 from "js-md5";
	export default {
		data() {
			return {
                titleTop: "产品体验-隐私号",
                Phone:"",
                mobileA:"",
                mobileB:"",
                newMobileA:"",
                newMobileB:"",
                dstVirtualNum:"",
                count:3,
                totalSum:'',
                typeid:true,
                // puzz:false,
                identifyCode:"",
                imgCode:"",
                popzindex:true,
                haibaoObj:{
                    icon:"Privacy.png",
                    txt:"隐私号",
                    id:1
                },
                loading:false,
			};
		},
		components: {
			Header,
            uniPopup,
            // Puzzle,
            SIdentify,
            suspendButton
        },
         onShareAppMessage(res) {
            console.log('转发')
			return {
				title: '汇聚丰富场景的云通信产品，中国东信邀您免费试用!',
				path: '/pages/index/index',
				imageUrl:"../../static/home/share.png",
				success: function(ress) {
                    console.log(ress)
                }
			}
		},
        onReady() {
            let that = this;
            that.$authorization()
            
            uni.getStorage({
                key: "phone",
                success: function(res) {
                    console.log(res.data);
                    if (res.data && res.data != undefined && res.data != "undefined") {
                        that.Phone = res.data;
                        that.$getRequestCount(1,res.data)
                            .then((response) => {
                                that.count=response.data.count
                                that.totalSum=response.data.initCount
                                that.refreshCode()
                            })
                    }
                }
            });   
        },
		methods: {
            phoneCall(phone){
                wx.makePhoneCall({
                    phoneNumber: phone,
                    success: function () {
                        console.log("成功拨打电话")
                    },
                    })
            },
            wailian(src) {
                console.log(src);
                uni.navigateTo({
                    url: "/pages/outside/outside?id="+src
                });
            },
            refreshCode(){
                let that=this
                let time = Math.round(new Date().getTime() / 1000).toString();
                wx.request({
					url: that.$reUrl+"/console/trialRequest.html?f=getVerification",
					header: {
						"content-type": "application/x-www-form-urlencoded",
						sign: md5("DONGXINYITONG-ONLINE!" + time),
                        signtime: time,
                        phone:that.Phone
					},
					method: "POST",
					success(ress) {
                        console.log(ress.data);
                        that.identifyCode=ress.data.data.verification
                    }
                })
            },
            open(){
                this.$refs.popup1.open()
            },
            event(data){
                console.log(data)
                // this.puzz=data.msg
                if(data==true){
                    console.log(this.popzindex)
                    this.popzindex=false
                }else{
                    console.log(this.popzindex)
                    this.popzindex=true
                }
            },
            wailian(src) {
                console.log(src);
                uni.navigateTo({
                    url: "/pages/outside/outside?id="+src
                });
            },
            handelChange(e){
                if(e.show==true){
                    console.log(this.popzindex)
                    this.popzindex=false
                }else{
                    console.log(this.popzindex)
                    this.popzindex=true
                }
            },
			generate() {
                console.log("提交")
                let time = Math.round(new Date().getTime() / 1000).toString();
                let that = this;
                if(that.mobileA==""){
                    uni.showToast({
                        title: "请输入手机号A",
                        icon:'none',
                        duration: 2000
                    });
                    return false;
                }
                if(that.mobileB==""){
                    uni.showToast({
                        title: "请输入手机号B",
                        icon:'none',
                        duration: 2000
                    });
                    return false;
                }
                if(that.mobileA.length!=11){
                    uni.showToast({
                        title: "请输入正确的手机号A",
                        icon:'none',
                        duration: 2000
                    });
                    return false;
                }
                if(that.mobileB.length!=11){
                    uni.showToast({
                        title: "请输入正确的手机号B",
                        icon:'none',
                        duration: 2000
                    });
                    return false;
                }
                if(that.imgCode==""){
                    uni.showToast({
                        title: "请填写验证码",
                        icon:'none',
                        duration: 2000
                    });
                    return false;
                }
                that.loading=true
				wx.request({
					url: that.$reUrl+"/console/trialRequest.html?f=privacyNum",
					data: {
                        mobileA: that.mobileA,
                        mobileB: that.mobileB,
                        verification:that.imgCode
					},
					header: {
						"content-type": "application/x-www-form-urlencoded",
						sign: md5("DONGXINYITONG-ONLINE!" + time),
                        signtime: time,
                        phone:that.Phone
					},
					method: "POST",
					success(ress) {
                        console.log(ress.data);
                        that.loading=false
                        if(ress.data.code==0){
                            let mobileA=that.mobileA
                            let mobile1  = mobileA.substr(0, 3) + '****' + mobileA.substr(7)
                            let mobileB=that.mobileB
                            let mobile2  = mobileB.substr(0, 3) + '****' + mobileB.substr(7)
                            that.newMobileA=mobile1
                            that.newMobileB=mobile2
                            that.dstVirtualNum=ress.data.data.dstVirtualNum
                            that.$getRequestCount(1,that.Phone)
                                .then((response) => {
                                    that.count=response.data.count
                                    that.typeid=false
                                    that.refreshCode()
                                    // setTimeout(()=>{
                                    //     that.puzz=false
                                    // },500)
                                })
                        }else{
                            uni.showToast({
                                title: ress.data.msg,
                                icon:'none',
                                duration: 2000
                            });
                        }
                            
					}
				})
			},
            backgen(){
                let that = this;
                that.typeid=true
                that.$getRequestCount(1,that.Phone)
                    .then((response) => {
                        that.count=response.data.count
                    })
            },
            back(){
                uni.navigateBack({
                    delta: 1,
                })
            }
		}
	}
</script>

<style lang="scss">

	.privacy {
		height: 100vh;
		position: relative;
		background: #f5f5f5;
		z-index: 1;

		.privacy_main {
			position: relative;
			margin-top: 5rpx;

			.banner {
				height: 350rpx;
				width: 100%;
				position: absolute;
				top: 0;
				left: 0;
				z-index: 2;

				.img {
					display: block;
					height: 100%;
					width: 100%;

				}
			}

			.privacy_center {
				position: relative;
				z-index: 2;
				.privacy_header {
					height: 240rpx;

					.title {
						padding: 70rpx 0 35rpx 50rpx;
						font-size: 34rpx;
                        font-weight: 600;
					}

					.xq_btn {
						width: 180rpx;
						height: 58rpx;
						display: flex;
						align-items: center;
						justify-content: center;
						background: #fff;
						border-radius: 25rpx;
						margin-left: 50rpx;
						font-size: 22rpx;
						color: #5c5c5c;

						.img {
							width: 8rpx;
							height: 12rpx;
							display: block;
							margin-left: 15rpx;
						}
					}
				}

				.privacy_from {
					width: 630rpx;
					margin: 0 20rpx;
					background: #fff;
					padding: 40rpx;
					border-radius: 7rpx;

					.top {
						display: flex;
						justify-content: flex-end;

						.yanshi {
							width: 155rpx;
							height: 44rpx;
							border: 1rpx solid #ff442e;
							display: flex;
							align-items: center;
							justify-content: center;
							font-size: 22rpx;
							color: #ff442e;
							border-radius: 9rpx;
							padding: 0;
							margin: 0;
							background-color: #fff;

							.img {
								width: 22rpx;
								height: 22rpx;
								display: block;
								margin-right: 5rpx;
							}

						}

						.yanshi::after {
							border: none;
						}

						.pop {
                            z-index: 9999;
							.txt {
								width: 600rpx;
								background: #fff;
								border-radius: 18rpx;
								padding: 40rpx;
								font-size: 24rpx;
								color: #797979;
								line-height: 30rpx;
                                z-index: 9999;
								._br {
									height: 10rpx;
								}
							}
						}

					}

					.privacy_input {
						display: flex;
						align-items: center;
						height: 73rpx;
						border-bottom: 1rpx solid #e9e9e9;
						padding-top: 50rpx;

						.left {
							display: flex;
							align-items: center;
							font-size: 24rpx;
							color: #5d5d5d;
							width: 191rpx;

							.img {
								width: 32rpx;
								height: 32rpx;
								display: block;
								margin: 0 5rpx;
							}
						}

						.srinput {
							font-size: 28rpx;
							color: #242424;
							line-height: 50rpx;
							height: 50rpx;
							display: block;
						}
					}
                    // .privacy_puzz{
                    //     border-bottom:none;
                    //     height: 80rpx;
                    // }
				}

				.privacy_footer {
					margin: 0 20rpx;
					margin-top: 36rpx;

					.hint {
						.title {
							display: flex;
							align-items: center;
							font-size: 22rpx;
							color: #606060;
							margin-bottom: 13rpx;

							.img {
								width: 43rpx;
								height: 42rpx;
								display: block;
							}
						}

						.txt {
							font-size: 20rpx;
							color: #797979;
							margin: 0 10rpx;
							letter-spacing: 2rpx
						}
					}

					.generate {
						background: #30a1fe;
						width: 690rpx;
						height: 90rpx;
						line-height: 90rpx;
						border-radius: 50rpx;
						margin-top: 47rpx;
						font-size: 30rpx;
						font-weight: 600;
						letter-spacing: 2rpx
					}

					.num {
						text-align: center;
						margin-top: 28rpx;
						font-size: 22rpx;
						letter-spacing: 2rpx;
						color: #909090;
					}
				}

                .popItem{
                    width: 630rpx;
                    margin: 0 20rpx;
                    background: #fff;
                    padding: 40rpx;
                    border-radius: 7rpx;
                    display: flex;
                    justify-content: center;
                    flex-direction: column;
                    align-items: center;
                    .popItem_top{
                        display: flex;
                        justify-content: center;
                        align-items: center;
                        font-size: 24rpx;
                        color: #242424;
                        .txt{
                            margin: 0 10rpx;
                            font-size: 26rpx;
                        }
                        text{
                            font-size: 24rpx;
                            color: #242424;
                            
                        }
                        .popItem_top_phone{
                            display: flex;
                            .img{
                                height: 32rpx;
                                width: 32rpx;
                                display: block;
                            }
                        }
                        
                    }
                    .popItem_t1{
                        margin-top: 40rpx;
                        margin-bottom: 50rpx;
                        font-size: 26rpx;
                        color: #242424;
                        letter-spacing: 1rpx
                    }
                    .popItem_t2{
                        font-size: 32rpx;
                        color: #242424;
                        font-weight: 600;
                        border-bottom: 1rpx dashed #505050;
                        letter-spacing: 1rpx
                    }
                    .popItem_t3{
                        font-size: 20rpx;
                        letter-spacing: 1rpx;
                        color:#797979;
                        margin-top: 24rpx;
                        margin-bottom: 40rpx;
                    }
                    .popItem_img{
                        .img{
                            width: 160rpx;
							height: 160rpx;
                            display: block;
                        }
                    }
                }
			}
		}
	}
    .butGather{
        display: flex;
        width: 100%;
        .generate{
            margin: 0 30rpx;
        }
        .generate1{
            background: #fff !important;
            border: 1rpx solid rgba(0,0,0,.2);
            color: rgba(0,0,0,0.85);
            margin-right: 0;
        }
        .generate1::after{
            border: none;
        }
    }
</style>
