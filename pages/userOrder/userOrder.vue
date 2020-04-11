<template>
	<view>
		
		<view class="topNavFix f5bj">
			<view class="tooling_indNav">
				<view class="list">
					<view class="item" :class="curr==1?'on':''" @click="changeCurr('1')">到店自提</view>
					<view class="item" :class="curr==2?'on':''" @click="changeCurr('2')">送货上门</view>
				</view>
			</view>
			
			<view class="orderNav whiteBj boxShaow color6 mgb15 flexRowBetween" v-show="curr==1">
				<view class="tt" :class="serviceNum==1?'on':''" @click="serviceChange('1')">全部</view>
				<view class="tt" :class="serviceNum==2?'on':''" @click="serviceChange('2')">待核销</view>
				<view class="tt" :class="serviceNum==3?'on':''" @click="serviceChange('3')">已核销</view>
			</view>
			
			<view class="orderNav whiteBj boxShaow color6 mgb15 flexRowBetween" v-show="curr==2">
				<view class="tt" :class="selfTakeNum==1?'on':''" @click="selfTakeChange('1')">全部</view>
				<view class="tt" :class="selfTakeNum==2?'on':''" @click="selfTakeChange('2')">待配送</view>
				<view class="tt" :class="selfTakeNum==3?'on':''" @click="selfTakeChange('3')">已配送</view>
				<view class="tt" :class="selfTakeNum==4?'on':''" @click="selfTakeChange('4')">已完成</view>
			</view>
		</view>
		
		<view class="topNavH"></view>
		
		
		<view v-show="curr==1">
			<view class="mglr4">
				<view class="proRow">
					<view class="item">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red">待核销</view>
						</view>
						<view class=" flexRowBetween">
							<view class="pic">
								<image src="../../static/images/shopping-img.png" mode=""></image>
							</view>
							<view class="infor">
								<view class="avoidOverflow2 fs13">墨西哥牛油果8枚单果200g左右</view>
								<view class="B-price flexRowBetween">
									<view class="price ftw">138</view>
									<view class="fs13">×1</view>
								</view>
							</view>
						</view>
						<view class="mgt15 flexRowBetween">
							<view class="">核销码：12<span class="fs12 color6 mgl15">自提时间 : 3月4日</span></view>
							<view class="hxEwm"  @click="hxEwmShow"><image src="../../static/images/the-orderl-img.png" mode=""></image></view>
						</view>
					</view>
					<view class="item">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red">已核销</view>
						</view>
						<view class="flexRowBetween">
							<view class="pic">
								<image src="../../static/images/shopping-img.png" mode=""></image>
							</view>
							<view class="infor">
								<view class="avoidOverflow2 fs13">墨西哥牛油果8枚单果200g左右</view>
								<view class="B-price flexRowBetween">
									<view class="price ftw">138</view>
									<view class="fs13">×1</view>
								</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<view v-show="curr==2">
			<view class="mglr4">
				<view class="proRow">
					<view class="item">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red">待配送</view>
						</view>
						<view class="proList flexRowBetween" v-for="(item,index) in proData" :key="index">
							<view class="pic">
								<image src="../../static/images/shopping-img.png" mode=""></image>
							</view>
							<view class="infor">
								<view class="avoidOverflow2 fs13">墨西哥牛油果8枚单果200g左右</view>
								<view class="B-price flexRowBetween">
									<view class="price">138</view>
									<view class="fs13">×1</view>
								</view>
							</view>
						</view>
					</view>
					<view class="item">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red">已配送</view>
						</view>
						<view class="proList flexRowBetween">
							<view class="pic">
								<image src="../../static/images/shopping-img.png" mode=""></image>
							</view>
							<view class="infor">
								<view class="avoidOverflow2 fs13">墨西哥牛油果8枚单果200g左右</view>
								<view class="B-price flexRowBetween">
									<view class="price">138</view>
									<view class="fs13">×1</view>
								</view>
							</view>
						</view>
						<view class="underBtn flexEnd mgt15">
							<view class="Bbtn">确认收货</view>
						</view>
					</view>
					<view class="item">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red">已完成</view>
						</view>
						<view class="flexRowBetween">
							<view class="pic">
								<image src="../../static/images/shopping-img.png" mode=""></image>
							</view>
							<view class="infor">
								<view class="avoidOverflow2 fs13">墨西哥牛油果8枚单果200g左右</view>
								<view class="B-price flexRowBetween">
									<view class="price">138</view>
									<view class="fs13">×1</view>
								</view>
							</view>
						</view>
					</view>
					
				</view>
			</view>
		</view>
		
		<!-- 无数据 -->
		<view class="nodata"><image src="../../static/images/nodata.png" mode=""></image></view>
		
		<!-- 核销码放大弹框 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="hxEwmShow" v-show="is_hxEwmShow">
			<view><image class="ewm" src="../../static/images/the-orderl-img.png" mode=""></image></view>
			<view class="closeBtn" @click="hxEwmShow">×</view>
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
				curr:1,
				serviceNum:1,
				selfTakeNum:1,
				is_hxEwmShow:false,
				proData:[{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeCurr(curr){
				const self = this;
				if(curr!= self.curr){
					self.curr = curr
				}
			},
			serviceChange(serviceNum){
				const self = this;
				if(serviceNum!= self.serviceNum){
					self.serviceNum = serviceNum
				}
			},
			selfTakeChange(selfTakeNum){
				const self = this;
				if(selfTakeNum!= self.selfTakeNum){
					self.selfTakeNum = selfTakeNum
				}
			},
			hxEwmShow(){
				const self = this;
				self.is_show = !self.is_show
				self.is_hxEwmShow = !self.is_hxEwmShow
			}
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/proRow.css";
	@import "../../assets/style/editInfor.css";
	page{background: #F5F5F5;}
	
	.topNavFix{width: 100%;height: 220rpx;position: fixed;top: 0rpx;right: 0;left: 0;box-sizing: border-box;z-index: 22;}
	.topNavH{height: 220rpx;}
	
	.tooling_indNav .list{height: 80rpx;}
	.tooling_indNav .list .item{line-height: 80rpx;}
	
	.proRow .item{margin-bottom: 30rpx;padding-top: 20rpx;}
	.proRow .item .pic{width: 160rpx;height: 160rpx;}
	.proRow .item .infor{width: 72%;height: 160rpx;}
	.proList{padding-bottom: 20rpx;}
	.proList:last-child{padding-bottom: 0;}
	
	.hxEwm{width: 85rpx;height: 85rpx;}
	
	.hxEwmShow{width: 70%;position: fixed;top: 45%;left: 50%;transform: translate(-50%,-50%);z-index: 50;}
	.hxEwmShow .ewm{width: 400rpx;height: 400rpx;display: block; margin: 0 auto;}
	.closeBtn{background: rgba(0,0,0,0.5);border: 0;top: auto;bottom: -130rpx;}
</style>
