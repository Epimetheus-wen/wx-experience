<template>
	<view class="twice">
		<Header :titleTop="titleTop"></Header>
		<view class="twice_main">
			<view class="banner">
				<image class="img" src="../../static/privacy/banner@1x.jpg" mode=""></image>
			</view>
			<view class="twice_center">
				<view class="twice_header">
					<view class="title">
						二次放号校验
					</view>
					<view class="xq_btn" @tap="wailian('product/numbercheck')">
						<text>产品详情</text>
						<image class="img" src="../../static/privacy/sanjiaoxing@1x.png" mode=""></image>
					</view>
				</view>
				<view class="twice_from">
					<view class="twice_input">
						<view class="left">
							<image class="img" src="../../static/privacy/number@1x.png" mode=""></image>
							<text>手机号码</text>
						</view>
						<input v-model="mobile" class="uni-input srinput" placeholder="输入手机号" />
					</view>
                    <view class="twice_input">
						<view class="left">
							<image class="img img1" src="../../static/privacy/date@1x.png" mode=""></image>
							<text>查询日期</text>
						</view>
						<view class="uni-list">
                            <view class="uni-list-cell">
                                <view class="uni-list-cell-db">
                                    <picker :class="date=='选择需要验证的日期'?'srinput_picker':'srinput'" mode="date" :value="date" :start="startDate" :end="endDate" @change="bindDateChange">
                                        <view class="uni-input">{{date}}</view>
                                    </picker>
                                </view>
                            </view>
                        </view>
					</view>
                    <view class="ts">验证在提供的日期之后，该手机号是否二次放号</view>
                    <view class="twice_input twice_puzz">
						<view class="left">
							<image class="img img1" src="../../static/privacy/safe@1x.png" mode=""></image>
							<text>安全验证</text>
						</view>
                        <input v-model="imgCode" style="width: 216rpx;" class="uni-input srinput" placeholder="请输入验证码" />
                        <view v-show="popzindex" @tap="refreshCode" class="right" style="width:160rpx;height:64rpx;overflow: hidden;margin-bottom: 10rpx;margin-left: 50rpx;"><SIdentify style="width:160rpx;height:64rpx" :identifyCode="identifyCode"></SIdentify></view>
                        <!-- <view class="puz"><Puzzle v-if="tank" @event='event'></Puzzle></view> -->
					</view>
                </view>
				<view class="twice_footer">
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
								手机号码：<text>{{newMobile}}</text>
							</view>
                            <view class="title">
								查询日期：<text>{{date}}</text>
							</view>
							<view class="txt">
                                检测结果：<text class="type_item">{{newText}}</text>
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
                    <suspendButton :haibaoObj='haibaoObj' @event='event' :refreshUrl="'twice'"></suspendButton>
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
                titleTop: "产品体验-二次放号校验",
                date: '选择需要验证的日期',
                Phone:"",
                mobile:"",
                newMobile:"",
                newText:"",
                count:0,
                // puzz:false,
                // tank:true
                identifyCode:"",
                imgCode:"",
                popzindex:true,
                haibaoObj:{
                    icon:"Check_number.png",
                    txt:"二次放号校验",
                    id:62
                },
                loading:false,
			};
		},
		components: {
			Header,
            uniPopup,
            // Puzzle
            SIdentify,
            suspendButton,
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
                        that.$getRequestCount(62,res.data)
                            .then((response) => {
                                console.log(response)
                                that.count=response.data.count
                                 that.refreshCode()
                            })
                    }
                }
            });   
        },
        computed:{
            startDate() {
            return this.getDate('start');
        },
            endDate() {
            return this.getDate('end');
        }
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
                if(that.mobile==""){
                    uni.showToast({
                        title: "请输入手机号",
                        icon:'none',
                        duration: 2000
                    });
                    return false;
                }
                if(that.mobile.length!=11){
                    uni.showToast({
                        title: "请输入正确的手机号",
                        icon:'none',
                        duration: 2000
                    });
                    return false;
                }
                if(that.date=="选择需要验证的日期"){
                    uni.showToast({
                        title: "请选择需要验证的日期",
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
					url: that.$reUrl+"/console/trialRequest.html?f=secondNumberVerify",
					data: {
                        phone: that.mobile,
                        date: that.date.replace("-","").replace("-",""),
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
                            let mobile=that.mobile
                            let mobile1  = mobile.substr(0, 3) + '****' + mobile.substr(7)
                            that.newMobile=mobile1
                            that.newText=ress.data.data.desc
                            that.$getRequestCount(62,that.Phone)
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
                that.$getRequestCount(62,that.Phone)
                    .then((response) => {
                        that.count=response.data.count
                    })
            },
            back(){
                uni.navigateBack({
                    delta: 1,
                })
            },
            bindDateChange: function(e) {
            this.date = e.target.value
            },
            getDate(type) {
            const date = new Date();
            let year = date.getFullYear();
            let month = date.getMonth() + 1;
            let day = date.getDate();

            if (type === 'start') {
                year = year - 60;
            } else if (type === 'end') {
                year = year + 2;
            }
            month = month > 9 ? month : '0' + month;;
            day = day > 9 ? day : '0' + day;
            return `${year}-${month}-${day}`;
        }
		}
	}
</script>

<style lang="scss">
	.twice {
		height: 100vh;
		position: relative;
		background: #f5f5f5;
		z-index: 1;

		.twice_main {
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

			.twice_center {
				position: relative;
				z-index: 2;
				.twice_header {
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

				.twice_from {
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
                    .ts{
                       color: #d4972c; 
                       font-size: 20rpx;
                       background: #fbf3e7;
                       height: 34rpx;
                       margin-top: 12rpx;
                       padding-left: 14rpx;
                       letter-spacing: 1rpx
                    }
					.twice_input {
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
						}
                        .srinput_picker{
                            font-size: 28rpx;
                             color: #989898
                        }
					}
                    .twice_puzz{
                        // border-bottom:none;
                        height: 80rpx;
                    }
				}

				.twice_footer {
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
                            font-size: 25rpx;
                            color: #797979;
                            text-align: center;
                            letter-spacing: 2rpx;
                            display: flex;
                            text{
                                width: 164rpx;
                                display: block;
                            }
                        }
                      
                        .txt{
                            width: 500rpx;
                            color:#242424;
                            font-size: 25rpx;
                            letter-spacing: 2rpx;
                            margin-top: 25rpx;
                            margin-bottom: 60rpx;
                            text-align: center;
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
