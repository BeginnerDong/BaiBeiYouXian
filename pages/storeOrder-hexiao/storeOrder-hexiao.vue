<template>
	<view>
		
		<view class="mglr4 pdt15">
			<view class="proRow">
				<view class="item">
					<view class="fs12 flexRowBetween mgb10">
						<view class="color9">交易时间：{{mainData.create_time}}</view>
						<view class="red">{{mainData.transport_status==0?'待核销':'已核销'}}</view>
					</view>
					<view class="flexRowBetween pdb15 borderB1">
						<view class="pic">
							<image  :src="mainData.orderItem&&mainData.orderItem[0]&&mainData.orderItem[0].snap_product&&
							mainData.orderItem[0].snap_product.mainImg&&mainData.orderItem[0].snap_product.mainImg[0]?mainData.orderItem[0].snap_product.mainImg[0].url:''"  mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow2 fs13">{{mainData.orderItem&&mainData.orderItem[0]&&mainData.orderItem[0].snap_product&&
						mainData.orderItem[0].snap_product?mainData.orderItem[0].snap_product.title:''}}</view>
							<view class="B-price flexRowBetween">
								<view class="price">{{mainData.price}}</view>
								<view class="fs13">×{{mainData.count}}</view>
							</view>
						</view>
					</view>
					<view class="pdt15 flexRowBetween fs13">
						<view class="color6">客户信息：</view>
						<view class="flexEnd">
							<view>{{mainData.name}}</view>
							<view class="mgl10">{{mainData.phone}}</view>
						</view>
					</view>
				</view>
			</view>
			
		</view>
		<view class="submitbtn" style="margin-top:160rpx;">
			<button class="btn" type="button" v-if="mainData.transport_status==0" @click="$Utils.stopMultiClick(orderUpdate)">确认核销</button>
			<button class="btn" type="button" v-if="mainData.transport_status==2">已核销</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				mainData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			orderUpdate() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const postData = {};
				postData.tokenFuncName = 'getStaffToken';
				postData.data = {
					transport_status:2,
				};
				postData.searchItem = {
					id:self.id,
					user_type:0
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('操作成功','none');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
			 },
			
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {
					searchItem:{
						code:self.id,
						user_type:0
					}
				};
				postData.tokenFuncName = 'getStaffToken'
				postData.getAfter = {
					orderItem:{
						tableName:'OrderItem',
						middleKey:'order_no',
						key:'order_no',
						searchItem:{
							status:1,
							user_type:0
						},
						condition:'='
					},
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];
					} else {
						self.$Utils.showToast(res.msg, 'none');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;}
	
	.proRow .item .pic{width: 160rpx;height: 160rpx;}
	.proRow .item .infor{width: 72%;height: 160rpx;}
	
	textarea{color: #222;padding: 30rpx;}
</style>
