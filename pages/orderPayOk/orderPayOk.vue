<template>
	<view>
		<view class="headTit center pr white">
			<image src="../../static/images/successfull-img.png" mode=""></image>
			<view class="tit fs18">您的单号：{{mainData.order_no?mainData.order_no:''}}</view>
		</view>
		
		<view class="pdlr4 okCont">
			<view class="okIcon"><image src="../../static/images/successfull-img1.png" mode=""></image></view>
			<view class="center mgb30">支付成功</view>
			
			<view class="seltBtn flexRowBetween fs14 color2 center" style="width:85%;margin:0 auto ;">
				<view class="btn" @click="Router.reLaunch({route:{path:'/pages/index/index'}})">继续购物</view>
				<button class="btn on" open-type="share">提醒老板接单</button>
			</view>
			<view class="fs12 mgt20 color6">点击上面的<span class="ftw pubColor">“提醒老板接单”</span>按钮，将提醒消息发送到该店客户微信，提醒老板及时处理您的订单</view>
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				score:'',
				wx_info:{},
				is_show:false,
				is_tips:false,
				mainData:{},
				Utils:this.$Utils,
				pay:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onShareAppMessage(ops) {
			console.log(ops)
			const self = this;
			if (ops.from === 'button') {
				return {
					title: '老板，刚在店里买的商品请接单！',
					path: '/pages/share-detail/share-detail?id='+self.mainData.id, //点击分享的图片进到哪一个页面
					
					success: function(res) {
						// 转发成功
						console.log("转发成功:" + JSON.stringify(res));
					},
					fail: function(res) {
						// 转发失败
						console.log("转发失败:" + JSON.stringify(res));
					}
				}
				console.log(ops.target)
			}else{
				return {
					title: '老板，刚在店里买的商品请接单！',
					path: '/pages/share-detail/share-detail?id='+self.mainData.id, //点击分享的图片进到哪一个页面
					
					success: function(res) {
						// 转发成功
						console.log("转发成功:" + JSON.stringify(res));
					},
					fail: function(res) {
						// 转发失败
						console.log("转发失败:" + JSON.stringify(res));
					}
				}
				console.log(ops.target)
			}
			
		},
		
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id:self.id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
		}
	};
</script>


<style>
	page{padding-bottom: 60rpx;}
	.headTit image{width: 100%;height: 270rpx;}
	.headTit .tit{position: absolute;top: 100rpx;left: 4%;right: 4%;z-index: 1;}
	.okIcon{width: 259rpx;height: 270rpx;margin: 60rpx auto 20rpx auto;}
	.okIcon image{width: 100%;height: 100%;}
	
	.seltBtn .btn{width: 260rpx;height: 70rpx;line-height: 70rpx;border-radius:40rpx;border:1px solid #ccc;}
	.seltBtn .btn.on{border-color: #E1211C;color: #E1211C;}
	button{
		background: none;
		line-height: 1.5;
		margin-left: 0;
		margin-right: 0;
		font-size: 15px;
		border-radius: 0;
	}
	button::after{
		border: none;
	}
	.button-hover {
		color: #000000;
		background: none;
	}
</style>
