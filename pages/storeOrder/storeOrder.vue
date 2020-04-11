<template>
	<view>
		
		<view class="topNavFix f5bj">
			<view class="orderNav whiteBj boxShaow color6 mgb15 flexRowBetween">
				<view class="tt" :class="serviceNum==1?'on':''" @click="serviceChange('1')">全部</view>
				<view class="tt" :class="serviceNum==2?'on':''" @click="serviceChange('2')">待核销</view>
				<view class="tt" :class="serviceNum==3?'on':''" @click="serviceChange('3')">已核销</view>
			</view>
		</view>
		<view class="topNavH"></view>
		
		<view class="R-fixIcon" @click="Router.navigateTo({route:{path:'/pages/storeOrder-hexiao/storeOrder-hexiao'}})"><image src="../../static/images/merchantsl-icon.png" mode=""></image></view>
		
		<view class="pdt15">
			<view class="mglr4">
				<view class="proRow">
					<view class="item">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red">待核销</view>
						</view>
						<view class="flexRowBetween pdb15 borderB1">
							<view class="pic">
								<image src="../../static/images/shopping-img.png" mode=""></image>
							</view>
							<view class="infor">
								<view class="avoidOverflow fs13">墨西哥牛油果8枚单果200g左右</view>
								<view class="fs12 color6 pdt5">自提时间：3月4号</view>
								<view class="B-price flexRowBetween">
									<view class="price">138.00</view>
									<view class="fs13">×1</view>
								</view>
							</view>
						</view>
						<view class="pdt15 flexRowBetween fs13">
							<view class="color6">客户信息：</view>
							<view class="flexEnd">
								<view>张单</view>
								<view class="mgl10">1567894567</view>
							</view>
						</view>
					</view>
					<view class="item">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red">已核销</view>
						</view>
						<view class="flexRowBetween pdb15 borderB1">
							<view class="pic">
								<image src="../../static/images/shopping-img.png" mode=""></image>
							</view>
							<view class="infor">
								<view class="avoidOverflow fs13">墨西哥牛油果8枚单果200g左右</view>
								<view class="B-price flexRowBetween">
									<view class="price">56.00</view>
									<view class="fs13">×1</view>
								</view>
							</view>
						</view>
						<view class="pdt15 flexRowBetween fs13">
							<view class="color6">客户信息：</view>
							<view class="flexEnd">
								<view>张单</view>
								<view class="mgl10">1567894567</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 无数据 -->
		<view class="nodata"><image src="../../static/images/nodata.png" mode=""></image></view>
		
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
				curr:1,
				serviceNum:1,
				selfTakeNum:1,
				deliveryNum:1,
				is_hxEwmShow:false
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			serviceChange(serviceNum){
				const self = this;
				if(serviceNum!= self.serviceNum){
					self.serviceNum = serviceNum
				}
			}
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;}
	
	.topNavFix{width: 100%;height: 80rpx;position: fixed;top:88rpx;right: 0;left: 0;box-sizing: border-box;z-index: 22;}
	.topNavH{height: 80rpx;}
	
	.proRow .item{margin-bottom: 30rpx;padding-top: 20rpx;}
	.proRow .item .pic{width: 160rpx;height: 160rpx;}
	.proRow .item .infor{width: 72%;height: 160rpx;}
	
	.R-fixIcon{width: 90rpx;height: 90rpx;position: fixed;bottom:30%;right: 30rpx;z-index: 66;}
	.R-fixIcon image{width: 100%;height: 100%;}
</style>
