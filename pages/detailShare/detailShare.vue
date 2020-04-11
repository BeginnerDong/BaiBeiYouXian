<template>
	<view>
		<view class="backBtn" @click="pre()">
			<image src="../../static/images/arrowL.png" mode=""></image>
		</view>
		
		<view class="sharePic">
			<image src="../../static/images/share-img.png" mode=""></image>
		</view>
		
		<view class="ewm">
			<image src="../../static/images/share-icon.png" mode=""></image>
		</view>
		
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				wx_info:{},
				is_show:false
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			pre(){
				this.$router.go(-1);
			},
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>
	page{padding-bottom:140rpx;}
	
	.backBtn{padding: 20rpx 30rpx;position: fixed;top: 40rpx;left: 0rpx; z-index: 2;}
	.backBtn image{width: 20rpx;height:36rpx;}
	.sharePic{position: fixed;top: 0;left: 0;right: 0;bottom: 0;z-index: 1;}
	
	.ewm{width: 200rpx;height: 200rpx;position: fixed;bottom: 40rpx;right: 40rpx; z-index:5;}
</style>
