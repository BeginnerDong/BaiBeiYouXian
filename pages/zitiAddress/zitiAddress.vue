<template>
	<view>
		<view class="myaddress-lis flexRowBetween" v-for="(item,index) in myaddressDate" :key="index"  @click="seltCurr(index)">
			<view class="ll">
				<view class="flex title">
					<view class="name flex"><image class="icon" src="../../static/images/the-pointl-icon.png" mode=""></image>中华世纪城智能柜机</view>
					<view class="mgl15 numb color6 fs13">15989876789</view>
				</view>
				
				<view class="adrs">陕西省西安市高新区大都荟</view>
			</view>
			<view class="rr flexEnd">
				<view class="seltBox">
					<image class="icon" :src="curr == index?'../../static/images/shoppimg-icon.png':'../../static/images/shoppimg-icon1.png'" mode=""></image>
				</view>
			</view>
		</view>
			
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				is_show:false,
				wx_info: {},
				curr:0,
				myaddressDate:[{},{},{},{}]
			}
		},

		onLoad(options) {
			uni.setStorageSync('canClick', true);
		},

		onShow() {
			const self = this;
			document.title = ''
		},

		methods: {
			seltCurr(index){
				const self = this;
				self.curr = index
			},
			getMainData() {
				const self = this;
				self.$apis.userGet(postData, callback);
			}
		}
	}
</script>

<style>
	/* @import "../../assets/style/seltAlert.css"; */
	/* page{ background: #f5f5f5;} */
	.myaddress-lis{padding:30rpx 4%;border-bottom: 20rpx solid #F5F5F5;background: #fff; }
	.myaddress-lis .ll{ width: 85%;}
	/* .myaddress-lis .name{ font-size: 28rpx; color: #222;} */
	.myaddress-lis .ll .title{flex-wrap: wrap;}
	.myaddress-lis .ll .icon{ width: 26rpx; height: 26rpx; display: inline-block; margin-right: 10rpx;}
	.myaddress-lis .adrs{ font-size: 26rpx;color: #666; line-height: 40rpx;padding-top: 16rpx;}
	
	.myaddress-lis .rr{ width: 15%;}
	.seltBox{ width:40rpx; height: 40rpx; display:block;}
</style>
