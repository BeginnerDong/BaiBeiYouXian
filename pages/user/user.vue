<template>
	<view>
		<view class="userHead pr">
			<view class="headbj"><image src="../../static/images/successfull-img.png" mode=""></image></view>
			<view class="infor radius10 boxShaow whiteBj flexCenter">
				<view class="photo">
					<view class="photo" style="overflow: hidden;"><open-data type="userAvatarUrl"></open-data></view>
				</view>
				<view>
					<view class="fs15 ftw"><open-data type="userNickName"></open-data></view>
					<!-- <view class="fs14 mgt10">15689876759</view> -->
				</view>
				
			</view>
		</view>
		
		<view class="twoBox flex mgt25 fs14 color6">
			<view class="item flexColumn" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">
				<view class="icon"><image src="../../static/images/about-icon.png" mode=""></image></view>
				<view>我的订单</view>
			</view>
			<view class="item flexColumn" @click="Router.navigateTo({route:{path:'/pages/zitiAddress/zitiAddress'}})">
				<view class="icon"><image src="../../static/images/about-icon1.png" mode=""></image></view>
				<view>自提点</view>
			</view>
			<view class="item flexColumn" @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
				<view class="icon"><image src="../../static/images/about-icon2.png" mode=""></image></view>
				<view>收货地址</view>
			</view>
			<view class="item flexColumn" @click="Router.navigateTo({route:{path:'/pages/user-ContactUs/user-ContactUs'}})">
				<view class="icon"><image src="../../static/images/about-icon3.png" mode=""></image></view>
				<view>联系我们</view>
			</view>
			<view class="item flexColumn" @click="Router.navigateTo({route:{path:'/pages/storeLogin/storeLogin'}})">
				<view class="icon"><image src="../../static/images/about-icon4.png" mode=""></image></view>
				<view>门店入口</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 140rpx;">
			<view class="btn" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">去购物</view>
		</view>
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/car/car'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">购物车</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3-a.png" />
				</view>
				<view class="text this-text">我的</view>
			</view>
		</view>
		<!--底部tab键 end-->
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				score:'',
				wx_info:{}
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			tel(){
				const self = this;
				uni.makePhoneCall({
					phoneNumber: '15623568989',
				})
			},

		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/navbar.css";
	page{padding-bottom: 180rpx;}
	.headbj{width: 100%;height: 270rpx;position: absolute;top: 0;right: 0;bottom: 0;left: 0; z-index: -1;}
	.userHead{padding-top: 70rpx;}
	.userHead .infor{height: 240rpx;margin: 0 4%;padding: 30rpx 4%;box-sizing: border-box;position:relative ;}
	
	.userHead .photo{width: 120rpx;height: 120rpx;border-radius: 50%;margin-right: 20rpx;box-sizing: border-box;}
	
	.twoBox{flex-wrap:wrap ;}
	.twoBox .item{width: 33.33%;padding: 30rpx 0;}
	.twoBox .item .icon{width: 60rpx;height: 60rpx;display: block;margin: 0 auto 20rpx auto;}
	.twoBox .item .icon image{width: 100%;height: 100%;}
</style>
