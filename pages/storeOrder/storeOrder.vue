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
		
		<view class="R-fixIcon" @click="scanCode()"><image src="../../static/images/merchantsl-icon.png" mode=""></image></view>
		
		<view class="pdt15">
			<view class="mglr4">
				<view class="proRow">
					<view class="item" v-for="(item,index) in mainData" :key="index">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color9">交易时间：{{item.create_time}}</view>
							<view class="red" v-if="item.transport_status==0">待核销</view>
							<view class="red" v-if="item.transport_status==2">已核销</view>
						</view>
						<view class="flexRowBetween pdb15 borderB1">
							<view class="pic">
								<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product
							&&item.orderItem[0].snap_product.mainImg&&item.orderItem[0].snap_product.mainImg[0]?item.orderItem[0].snap_product.mainImg[0].url:''" mode=""></image>
							</view>
							<view class="infor">
								<view class="avoidOverflow fs13">{{item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product
							&&item.orderItem[0].snap_product?item.orderItem[0].snap_product.title:''}}</view>
								<view class="B-price flexRowBetween">
									<view class="price">{{item.price?item.price:''}}</view>
									<view class="fs13">×{{item.price?item.count:''}}</view>
								</view>
							</view>
						</view>
						<view class="pdt15 flexRowBetween fs13">
							<view class="color6">客户信息：</view>
							<view class="flexEnd">
								<view>{{item.name?item.name:'暂无'}}</view>
								<view class="mgl10">{{item.phone?item.phone:'暂无'}}</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 无数据 -->
		<view class="nodata" v-if="mainData.length==0"><image src="../../static/images/nodata.png" mode=""></image></view>
		<view class="submitbtn">
			<view class="btn" @click="loginOff" style="position: fixed;margin-left: 10%;bottom: 10%;">退出登录</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				
				serviceNum:1,
				
				mainData:[],
				searchItem:{
					pay_status:1,
					type:1,
					transport_type:1,
					user_type:0
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			//self.$Utils.loadAll(['getMainData'], self);
		},
		
		onShow() {
			const self = this;
			self.getMainData(true)
		},
		
		methods: {
			
			loginOff(){
				const self = this;
				uni.removeStorageSync('staffInfo');
				uni.removeStorageSync('staffToken');
				self.Router.redirectTo({route:{path:'/pages/user/user'}})
			},
			
			scanCode(){
				const self = this;
				uni.scanCode({
				    success: function (res) {
				        self.Router.navigateTo({route:{path:'/pages/storeOrder-hexiao/storeOrder-hexiao?id='+res.result}})
				        console.log('条码内容：' + res.result);
				    }
				});
			},
			
			serviceChange(serviceNum){
				const self = this;
				if(serviceNum!= self.serviceNum){
					self.serviceNum = serviceNum
					if(self.serviceNum==1){
						delete self.searchItem.transport_status
					}else if(self.serviceNum==2){
						self.searchItem.transport_status = 0
					}else if(self.serviceNum==3){
						self.searchItem.transport_status = 2
					}
					self.getMainData(true)
				}
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getStaffToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.searchItem.shop_no = uni.getStorageSync('staffInfo').user_no;
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
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;}
	
	.topNavFix{width: 100%;height: 80rpx;position: fixed;top:0;right: 0;left: 0;box-sizing: border-box;z-index: 22;}
	.topNavH{height: 80rpx;}
	
	.proRow .item{margin-bottom: 30rpx;padding-top: 20rpx;}
	.proRow .item .pic{width: 160rpx;height: 160rpx;}
	.proRow .item .infor{width: 72%;height: 160rpx;}
	
	.R-fixIcon{width: 90rpx;height: 90rpx;position: fixed;bottom:30%;right: 30rpx;z-index: 66;}
	.R-fixIcon image{width: 100%;height: 100%;}
</style>
