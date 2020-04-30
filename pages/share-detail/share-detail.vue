<template>
	<view>
		<view class="mglr4 mgt15">
			<view class="radius10 whiteBj oh">
				<view class="pdlr4 HeadState flexRowBetween radius10 whiteBj">
					<view class="flex red fs14" v-if="mainData.transport_status==0"><image style="width: 30rpx; height: 30rpx;margin-right: 10rpx;" src="../../static/images/remindl-icon.png" 
					mode=""></image>待提货</view>
					<view class="flex red fs14" v-if="mainData.transport_status==2"><image style="width: 30rpx; height: 30rpx;margin-right: 10rpx;" src="../../static/images/remindl-icon.png"
					mode=""></image>已提货</view>
					<view class="flexEnd">
						<view class="flexEnd">单号：<span class="red">{{mainData.order_no}}</span></view>
					</view>
				</view>
				<view class="w" style="height: 10rpx;">
					<image src="../../static/images/remindl-img.png" mode=""></image>
				</view>
			</view>
			
			<view class="whiteBj radius10 pdlr4 mgt15">
				<view class="pdtb15 fs13">
					<view>收货人：{{mainData.name?mainData.name:''}} {{mainData.phone?mainData.phone:''}}</view>
					<view class="flexRowBetween pdtb10">
						<view class="red">自提点：{{mainData.shop&&mainData.shop.name?mainData.shop.name:''}}</view>
						<view class="color6">{{mainData.shop&&mainData.shop.phone?mainData.shop.phone:''}}</view>
					</view>
					<view class="color6">地址：{{mainData.shop&&mainData.shop.address?mainData.shop.address:''}}</view>
				</view>
			</view>
			
			<view class="proRow mgt15">
				<view class="item"  v-for="(item,index) in mainData.child" :key="index">
					<view class="flexRowBetween">
						<view class="pic">
							<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product
							&&item.orderItem[0].snap_product.mainImg&&item.orderItem[0].snap_product.mainImg[0]?item.orderItem[0].snap_product.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow2 fs14">{{item.title}}</view>
							<view class="B-price">
								<view class="fs12 red">{{Utils.timeto(item.pickup_time,'md')}}</view>
								<view class="flexRowBetween">
									<view class="price fs16 ftw">{{item.price}}</view>
									<view class="fs13">×{{item.count}}</view>
								</view>
							</view>
						</view>
					</view>
					<view class="flexEnd pdt15 fs13">共{{item.count}}件商品<span class="mgl10">实付金额:</span><span class="price fs15 ftw">{{item.price}}</span></view>
				</view>
			</view>	
		
			<view class="whiteBj radius10 pdlr4 mgt15">
				<view class="pdtb15 fs13 color6">
					<view class="mgb10">订单编号：{{mainData.order_no?mainData.order_no:''}}</view>
					<view>下单时间：{{mainData.create_time?mainData.create_time:''}}</view>
				</view>
			</view>
			
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				mainData: {},
				Utils:this.$Utils,
			}
		},

		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},

		methods: {

		

			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id: self.id,
				};
				postData.getAfter = {
					shop: {
						tableName: 'UserInfo',
						middleKey: 'shop_no',
						key: 'user_no',
						searchItem: {
							status: 1,
							user_type: 1
						},
						condition: '=',
						info: ['name', 'address','phone']
					}
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
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;padding-bottom: 60rpx;}
	.underBtn{border-top:0;padding-bottom: 0;}
	.HeadState{height: 142rpx;box-sizing: border-box;padding: 14rpx 4% 0 4%;}
	
	.proRow .item .pic{width: 160rpx; height: 160rpx;border-radius: 10rpx;overflow: hidden;}
	.proRow .item .pic image{width: 100%;height: 100%; display: block;}
	.proRow .item .infor{ width:72%;height: 160rpx;position: relative;}
</style>
