<template>
	<view class="bankCardThree">
		<Header :titleTop="titleTop"></Header>
		<view class="bankCardThree_main">
			<view class="banner">
				<image class="img" src="../../static/privacy/banner@1x.jpg" mode=""></image>
			</view>
			<view class="bankCardThree_center">
				<view class="bankCardThree_header">
					<view class="title">
						银行卡三要素核验
					</view>
					<view class="xq_btn" @tap="wailian('product/identitycheck')">
						<text>产品详情</text>
						<image class="img" src="../../static/privacy/sanjiaoxing@1x.png" mode=""></image>
					</view>
				</view>
				<view class="bankCardThree_from">
                    <view class="bankCardThree_input">
						<view class="left">
							<image class="img img1" src="../../static/privacy/name@1x.png" mode=""></image>
							<text>姓名</text>
						</view>
						<input v-model="name" class="uni-input srinput" placeholder="输入姓名" />
					</view>
                    <view class="bankCardThree_input">
						<view class="left">
							<image class="img img1" src="../../static/privacy/id@1x.png" mode=""></image>
							<text>身份证号</text>
						</view>
						<input v-model="idCode"  class="uni-input srinput" placeholder="输入身份证号" />
					</view>
					<view class="bankCardThree_input">
						<view class="left">
							<image class="img img1" src="../../static/privacy/bank@1x.png" mode=""></image>
							<text>银行卡号</text>
						</view>
						<input v-model="bankCode" class="uni-input srinput" placeholder="请填写银联银行卡号" />
					</view>
                    <view class="bankCardThree_input bankCardThree_puzz">
						<view class="left">
							<image class="img img1" src="../../static/privacy/safe@1x.png" mode=""></image>
							<text>安全验证</text>
						</view>
                        <input v-model="imgCode" style="width: 216rpx;" class="uni-input srinput" placeholder="请输入验证码" />
                        <view v-show="popzindex" @tap="refreshCode" class="right" style="width:160rpx;height:64rpx;overflow: hidden;margin-bottom: 10rpx;margin-left: 50rpx;"><SIdentify style="width:160rpx;height:64rpx" :identifyCode="identifyCode"></SIdentify></view>
                        <!-- <view class="puz"><Puzzle v-if="tank" @event='event'></Puzzle></view> -->
					</view>

				</view>
				<view class="bankCardThree_footer">
					<view class="hint">
						<view class="title">
							<image class="img" src="../../static/privacy/xxx@1x.png" mode=""></image><text>隐私承诺</text>
						</view>
						<view class="txt">
							中国东信绝不会将您的手机号泄露给任何人，您所录入的信息只会用于该产品试用，可放心输入。
						</view>
					</view>
					<button class="generate" type="primary" :loading='loading' :disabled='loading' @tap="generate"  v-text="loading?' 正在发送中':'立即检测'">立即检测</button>
					<uni-popup class="pop" ref="popup1" type="center" @change='handelChange'>
						<view class="center">
							<image class="img" src="../../static/privacy/success@1x.png" mode=""></image>
							<view class="title">
								姓&emsp;&emsp;名：<text>{{name}}</text>
							</view>
                            <view class="title">
							    身份证号：<text>{{newidCode}}</text>
							</view>
                            <view class="title">
							    银行卡号：<text>{{newbankCode}}</text>
							</view>
							<view class="txt">
                                查询结果：<text class="type_item">{{newText}}</text>
							</view>
                            <!-- <button class="generate" type="primary" @tap="backgen">再次试用</button> -->
                            <view class="butGather">
                                <button class="generate generate1" type="primary" @tap="back">不再试用</button>
                                <button class="generate" type="primary" @tap="backgen">再次试用</button>
                            </view>
                            <view class="num">剩余可试用：<text>{{count}}</text>次</view>
						</view>
					</uni-popup>
					<view class="num">剩余可试用：<text>{{count}}</text>次</view>
                    <suspendButton :haibaoObj='haibaoObj' @event='event' :refreshUrl="'bankCardThree'"></suspendButton>
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
    import md5 from "js-md5";
    // import Puzzle from "../../components/puzzle.vue";
    import SIdentify from "../../components/identify";
	export default {
		data() {
			return {
                titleTop: "产品体验-银行卡三要素核验",
                Phone:"",
                name:"",
                idCode:"",
                bankCode:"",
                newidCode:"",
                newbankCode:"",
                newText:"",
                count:3,
                // puzz:false,
                // tank:true
                identifyCode:"",
                imgCode:"",
                popzindex:true,
                haibaoObj:{
                    icon:"identity.png",
                    txt:"银行卡三要素核验",
                    id:73
                },
                loading:false,
			};
		},
		components: {
			Header,
            uniPopup,
            // Puzzle
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
                    // console.log(res.data);
                    if (res.data && res.data != undefined && res.data != "undefined") {
                        that.Phone = res.data;
                        that.$getRequestCount(73,res.data)
                            .then((response) => {
                                console.log(response)
                                that.count=response.data.count
                                that.refreshCode()
                            })
                    }
                }
            });   
        },
		methods: {
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
			generate(){
                console.log("提交")
                let time = Math.round(new Date().getTime() / 1000).toString();
                let that = this;
                if(that.name==""){
                    uni.showToast({
                        title: "请输入姓名",
                        icon:'none',
                        duration: 2000
                    });
                    return false;
                }
                if(that.idCode==""){
                    uni.showToast({
                        title: "请输入身份证号",
                        icon:'none',
                        duration: 2000
                    });
                    return false;
                }
                if(that.bankCode==""){
                    uni.showToast({
                        title: "请输入银行卡号",
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
					url: that.$reUrl+"/console/trialRequest.html?f=identityVerifyThreeBankCard",
					data: {
                        idcard: that.idCode,
                        name: that.name,
                        bankcard:that.bankCode,
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
                        if(ress.data.respCode=='000'){
                            let idCode=that.idCode
                            let idCode1  = idCode.substr(0, 6) + '****' + idCode.substr(14)
                            that.newidCode=idCode1
                            let bankCode=that.bankCode
                            let bankCode1  = bankCode.substr(0, 4) + '********' + bankCode.substr(-4)
                            that.newbankCode=bankCode1
                            that.newText=ress.data.data.desc
                            that.$getRequestCount(73,that.Phone)
                                .then((response) => {
                                    that.count=response.data.count
                                    that.$refs.popup1.open()
                                    that.refreshCode()
                                    // that.tank=false
                                    // setTimeout(()=>{
                                    //     that.tank=true
                                    //     that.puzz=false
                                    // },500)
                                })
                        }else{
                            uni.showToast({
                                title: ress.data.errorMsg,
                                icon:'none',
                                duration: 2000
                            });
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
                that.$refs.popup1.close()
                that.$getRequestCount(73,that.Phone)
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
	.bankCardThree {
		height: 100vh;
		position: relative;
		background: #f5f5f5;
		z-index: 1;

		.bankCardThree_main {
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

			.bankCardThree_center {
				position: relative;
				z-index: 2;
				.bankCardThree_header {
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

				.bankCardThree_from {
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
							.txt {
								width: 600rpx;
								background: #fff;
								border-radius: 18rpx;
								padding: 40rpx;
								font-size: 24rpx;
								color: #797979;
								line-height: 30rpx;

								._br {
									height: 10rpx;
								}
							}
						}

					}

					.bankCardThree_input {
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
								width: 22rpx;
								height: 32rpx;
								display: block;
								margin: 0 10rpx;
							}
                            .img1{
                                width: 32rpx;
                                margin: 0 5rpx;
                            }
						}

						.srinput {
							font-size: 28rpx;
							color: #242424;
                            z-index: 1;
						}
					}
                    .bankCardThree_puzz{
                        // border-bottom:none;
                        height: 80rpx;
                    }
				}

				.bankCardThree_footer {
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
                    .center{
                        background: #fff;
                        border-radius: 18rpx;
                        width: 680rpx;
                        position: relative;
                        padding-top: 116rpx;
                        padding-bottom: 60rpx;
                        display: flex;
                        flex-direction: column;
                        align-items: center;
                        z-index: 999999;
                        .img{
                            width: 160rpx;
                            height: 160rpx;
                            display: block;
                            position:absolute;
                            left: 0;
                            right: 0;
                            top: -50rpx;
                            margin: auto;
                        }
                        .title{
                            width: 400rpx;
                            font-size: 25rpx;
                            color: #797979;
                            text-align: left;
                            letter-spacing: 2rpx;
                            margin-bottom:17rpx;
                        }
                        .txt{
                            width: 400rpx;
                            color:#242424;
                            font-size: 25rpx;
                            letter-spacing: 2rpx;
                            // margin-top: 25rpx;
                            margin-bottom: 60rpx;
                            text-align: left;
                        }
                        .generate{
                            width: 580rpx;
                            margin-top: 0rpx;
                        }
                        .type_item{

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
            margin: 0 50rpx;   
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
