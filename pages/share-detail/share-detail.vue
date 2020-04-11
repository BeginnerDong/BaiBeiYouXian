<template>
	<view>
		<view class="mglr4 mgt15">
			<view class="radius10 whiteBj oh">
				<view class="pdlr4 HeadState flexRowBetween radius10 whiteBj">
					<view class="flex red fs14"><image style="width: 30rpx; height: 30rpx;margin-right: 10rpx;" src="../../static/images/remindl-icon.png" mode=""></image>待提货</view>
					<view class="flexEnd">
						<view class="flexEnd">提货单号：<span class="red">8</span></view>
					</view>
				</view>
				<view class="w" style="height: 10rpx;">
					<image src="../../static/images/remindl-img.png" mode=""></image>
				</view>
			</view>
			
			<view class="whiteBj radius10 pdlr4 mgt15">
				<view class="pdtb15 fs13">
					<view>收货人：张丹 15623565689</view>
					<view class="flexRowBetween pdtb10">
						<view class="red">自提点：门店名称门店名称</view>
						<view class="color6">15689893456</view>
					</view>
					<view class="color6">地址：陕西省西安市高新区高新大都会A座</view>
				</view>
			</view>
			
			<view class="proRow mgt15">
				<view class="item">
					<view class="flexRowBetween">
						<view class="pic">
							<image src="../../static/images/shopping-img.png" mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow2 fs14">墨西哥牛油果8枚单果200g左右</view>
							<view class="B-price">
								<view class="fs12 red">12月21日 16:00提货</view>
								<view class="flexRowBetween">
									<view class="price fs16 ftw">126</view>
									<view class="fs13">×1</view>
								</view>
							</view>
						</view>
					</view>
					<view class="flexEnd pdt15 fs13">共1件商品<span class="mgl10">实付金额:</span><span class="price fs15 ftw">56</span></view>
				</view>
			</view>	
		
			<view class="whiteBj radius10 pdlr4 mgt15">
				<view class="pdtb15 fs13 color6">
					<view class="mgb10">订单编号：5522256553326567</view>
					<view>下单时间：2020-03-05 17:20:40</view>
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
				score:'',
				wx_info:{}
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
		}
	};
</script>

<style>
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;padding-bottom: 60rpx;}
	.underBtn{border-top:0;padding-bottom: 0;}
	.HeadState{height: 142rpx;box-sizing: border-box;padding: 14rpx 4% 0 4%;}
	
	.proRow .item .pic{width: 160rpx; height: 160rpx;border-radius: 10rpx;overflow: hidden;}
	.proRow .item .pic image{width: 100%;height: 100%; display: block;}
	.proRow .item .infor{ width:72%;height: 160rpx;position: relative;}
</style>
