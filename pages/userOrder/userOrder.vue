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
		
		<view class="topNavH" style="height: 250rpx;"></view>
		
		
		<view v-show="curr==1">
			<view class="mglr4">
				<view class="proRow">
					
					<view class="item" v-for="(item,index) in mainData" :key="index">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color9">交易时间：{{item.create_time}}</view>
							<view class="red" v-if="item.transport_status==1">未核销</view>
							<view class="red" v-if="item.transport_status==2">已核销</view>
						</view>
						<view class=" flexRowBetween">
							<view class="pic">
								<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product
							&&item.orderItem[0].snap_product.mainImg&&item.orderItem[0].snap_product.mainImg[0]?item.orderItem[0].snap_product.mainImg[0].url:''" mode=""></image>
							</view>
							<view class="infor">
								<view class="avoidOverflow2 fs13">{{item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product
							&&item.orderItem[0].snap_product?item.orderItem[0].snap_product.title:''}}</view>
								<view class="B-price flexRowBetween">
									<view class="price ftw">{{item.unit_price?item.unit_price:''}}</view>
									<view class="fs13">×{{item.count?item.count:''}}</view>
								</view>
							</view>
						</view>
						<view class="mgt15 flexRowBetween" v-if="item.transport_status==0">
							<view class="">核销码：{{item.code}}<span class="fs12 color6 mgl15">自提时间 : {{Utils.timeto(item.pickup_time*1000,'md')}}</span></view>
							<view class="hxEwm"  @click="hxEwmShow(index)"><image :src="item.qrcode" mode=""></image></view>
						</view>
					</view>
					
					
				</view>
			</view>
		</view>
		
		<view v-show="curr==2">
			<view class="mglr4">
				<view class="proRow">
					<view class="item" v-for="(item,index) in mainData" :key="index">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color9">交易时间：{{item.create_time}}</view>
							<view class="red" v-if="item.transport_status==0">待配送</view>
							<view class="red" v-if="item.transport_status==1">已配送</view>
							<view class="red" v-if="item.transport_status==2">已完成</view>
						</view>
						<view class="proList flexRowBetween" v-for="(c_item,c_index) in item.child" :key="index">
							<view class="pic">
								<image :src="c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product
								&&c_item.orderItem[0].snap_product.mainImg&&c_item.orderItem[0].snap_product.mainImg[0]?c_item.orderItem[0].snap_product.mainImg[0].url:''" mode=""></image>
							</view>
							<view class="infor">
								<view class="avoidOverflow2 fs13">{{c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product
								&&c_item.orderItem[0].snap_product?c_item.orderItem[0].snap_product.title:''}}</view>
								<view class="B-price flexRowBetween">
									<view class="price">{{c_item.unit_price?c_item.unit_price:''}}</view>
									<view class="fs13">×{{c_item.count?c_item.count:''}}</view>
								</view>
							</view>
						</view>
						<view class="underBtn flexEnd mgt15" v-if="item.transport_status==1">
							<view class="Bbtn">确认收货</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 无数据 -->
		<view class="nodata" v-if="mainData.length==0"><image src="../../static/images/nodata.png" mode=""></image></view>
		
		<!-- 核销码放大弹框 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="hxEwmShow" v-show="is_hxEwmShow">
			<view><image class="ewm" :src="mainData[previewIndex]?mainData[previewIndex].qrcode:''" mode=""></image></view>
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
				proData:[{},{},{}],
				searchItem:{
					pay_status:1,
					transport_type:1,
					type:1
				},
				mainData:[],
				Utils:this.$Utils,
				previewIndex:-1
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
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no;
				if(self.curr==1){
					postData.getAfter = {
						orderItem:{
							tableName:'OrderItem',
							middleKey:'order_no',
							key:'order_no',
							searchItem:{
								status:1
							},
							condition:'='
						}
					};
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
			
			changeCurr(curr){
				const self = this;
				if(curr!= self.curr){
					self.curr = curr;
					self.searchItem.transport_status = ['in',[0,1,2]]
					self.searchItem.transport_type = self.curr;
					if(self.curr==2){
						self.searchItem.type=6
					}else if(self.curr==1){
						self.searchItem.type=1
					}
					self.getMainData(true)
				}
			},
			
			serviceChange(serviceNum){
				const self = this;
				if(serviceNum!= self.serviceNum){
					self.serviceNum = serviceNum;
					if(self.serviceNum==1){
						self.searchItem.transport_status = ['in',[0,1,2]]
					}else if(self.serviceNum==2){
						self.searchItem.transport_status = 0
					}else if(self.serviceNum==3){
						self.searchItem.transport_status = 2
					}
					self.getMainData(true)
				}
			},
			
			selfTakeChange(selfTakeNum){
				const self = this;
				if(selfTakeNum!= self.selfTakeNum){
					self.selfTakeNum = selfTakeNum
					if(self.selfTakeNum==1){
						self.searchItem.transport_status = ['in',[0,1,2]]
					}else if(self.selfTakeNum==2){
						self.searchItem.transport_status = 0
					}else if(self.selfTakeNum==3){
						self.searchItem.transport_status = 1
					}else if(self.selfTakeNum=4){
						self.searchItem.transport_status = 2
					}
					self.getMainData(true)
				}
			},
			
			hxEwmShow(index){
				const self = this;
				console.log(index)
				if(index||index==0){
					self.previewIndex = index
				};
				self.is_show = !self.is_show
				self.is_hxEwmShow = !self.is_hxEwmShow
			},
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
