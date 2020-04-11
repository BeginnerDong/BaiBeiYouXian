<template>
	<view>
		
		<view class="banner-box">
			<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-active-color="#757575">
				<block v-for="(item,index) in labelData" :key="index">
					<swiper-item class="swiper-item">
						<image :src="item" class="slide-image" />
					</swiper-item>
				</block>
			</swiper>
		</view>
		<view class="mglr4 pdtb15">
			<view class="fs16 ftw">墨西哥牛油果8枚单果200g左右</view>
			<view class="color6 fs12 pdt5">提货时间：3月21日</view>
			<view class="fs16 ftw red mgt10 ftw">￥126</view>
		</view>
		
		<view class="f5H10"></view>
		
		
		<view class="pdt15" >
			<view class="mglr4 xqInfor">
				<view class="fs15 ftw pdb20">商品规格描述</view>
				<view class="cont fs14 center">
					<view>管理客服电话</view>
					<view>悲愤交加鹤骨鸡肤供货</view>
					<view>方点击可供货方都拉黑干活的放假</view>
					<view>价格过节费考虑到加工费</view>
					<view><image class="w" src="../../static/images/detsilsl-img1.png" mode="widthFix"></image></view>
					<view>管理客服电话</view>
					<view>悲愤交加鹤骨鸡肤供货</view>
					<view>方点击可供货方都拉黑干活的放假</view>
					<view>价格过节费考虑到加工费</view>
				</view>
			</view>
		</view>
		
		<!-- 预算中 -->
		<view class="xqbotomBar center" style="height: 100rpx;">
			<view class=" flex fs12">
				<view class="ite flexColumn" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
					<image src="../../static/images/detsilsl-icon1.png" mode=""></image>
					<view class="mgt5">首页</view>
				</view>
				<view class="ite flexColumn" @click="Router.navigateTo({route:{path:'/pages/detailShare/detailShare'}})">
					<image src="../../static/images/detsilsl-icon3.png" mode=""></image>
					<view class="mgt5">分享</view>
				</view>
			</view>
			<view class="payBtn" style="width: 360rpx;">预售中</view>
		</view>
		
		<!-- 已售罄 -->
		<view class="xqbotomBar center" style="height: 100rpx;">
			<view class=" flex fs12">
				<view class="ite flexColumn" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
					<image src="../../static/images/detsilsl-icon1.png" mode=""></image>
					<view class="mgt5">首页</view>
				</view>
				<view class="ite flexColumn" @click="Router.navigateTo({route:{path:'/pages/detailShare/detailShare'}})">
					<image src="../../static/images/detsilsl-icon3.png" mode=""></image>
					<view class="mgt5">分享</view>
				</view>
			</view>
			<view class="payBtn" style="width: 360rpx;background: #bdbcbc;">已售罄</view>
		</view>
		
		<!-- 抢购中 -->
		<view class="xqbotomBar center" style="height: 100rpx;">
			<view class=" flex fs12">
				<view class="ite flexColumn" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
					<image src="../../static/images/detsilsl-icon1.png" mode=""></image>
					<view class="mgt5">首页</view>
				</view>
				<view class="ite flexColumn">
					<image src="../../static/images/detsilsl-icon2.png" mode=""></image>
					<view class="mgt5">加入购物车</view>
				</view>
				<view class="ite flexColumn" @click="Router.navigateTo({route:{path:'/pages/detailShare/detailShare'}})">
					<image src="../../static/images/detsilsl-icon3.png" mode=""></image>
					<view class="mgt5">分享</view>
				</view>
			</view>
			<view class="payBtn" @click="Router.navigateTo({route:{path:'/pages/orderConfim/orderConfim'}})">立即购买</view>
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
				is_show:false,
				labelData: [
					"../../static/images/detsilsl-img.png",
					"../../static/images/detsilsl-img.png",
					"../../static/images/detsilsl-img.png"
				],
				curr:1,
				messageData:[{},{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
				}
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
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/detail.css";
	page{padding-bottom:140rpx;}
	
	.swiper-box {height: 400rpx;box-sizing: border-box;overflow: hidden;}
	.swiper-box swiper-item{width: 100%;box-sizing: border-box;overflow: hidden;}
	.swiper-box swiper-item image{width: 100%;height: 100%;box-sizing: border-box;}
	
	.orderNav .tt.on{font-weight: bold;}
	.orderNav .tt.on::after{bottom:6rpx;}
	
	.shareBtn{background: #F5F5F5;width: 120rpx;height: 44rpx;border-radius: 25rpx 0 0 25rpx;}
	
	.xqbotomBar{z-index: 45;}
	
	
</style>
