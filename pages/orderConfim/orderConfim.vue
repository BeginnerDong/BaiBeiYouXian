<template>
	<view>
		<view class="mglr4 mgt15">
			
			<view class="whiteBj radius10 boxShaow oh mgb15 pdb15">
				<view class="pdlr4pdb15">
					<view class="flex seltTake center fs15 color6">
						<view class="btn" :class="curr==1?'on':''" @click="currChange('1')">到店自提</view>
						<view class="btn" :class="curr==2?'on':''" @click="currChange('2')">送货上门</view>
					</view>
				
					<view class="pdlr4 radius10" v-show="curr==1">
						<view class="flexRowBetween pdtb15 borderB1" v-if="shopData.name"
						@click="Router.navigateTo({route:{path:'/pages/zitiAddress/zitiAddress'}})">
							<view class="fs14 color6">
								<view class="flex mgt5">
									<view class="mgr15 color2">{{shopData.name}}</view>
									<view>{{shopData.phone}}</view>
								</view>
								<view class="mgt5">{{shopData.address}}</view>
							</view>
							<view class="flexEnd" style="width: 20%;">
								<image class="arrowR" src="../../static/images/addressl-icon2.png" mode=""></image>
							</view>
						</view>
						<view class="flexRowBetween  pdtb15 borderB1" v-else
						@click="Router.navigateTo({route:{path:'/pages/zitiAddress/zitiAddress'}})">
							<view class="fs14 color6 flex">
								<view style="width: 36rpx;height: 36rpx;margin-right: 10rpx;"><image style="" src="../../static/images/shoppimg-icon2.png" mode=""></image></view>
								<view class="red">选择自提门店</view>
							</view>
							<view class="flexEnd" style="width: 20%;">
								<image class="arrowR" src="../../static/images/addressl-icon2.png" mode=""></image>
							</view>
						</view>
						<view class="flexRowBetween pdt15">
							<view class="editMsg"><input type="text" v-model="orderInfo.name" placeholder="请输入姓名"  placeholder-class="placeholder" /></view>
							<view class="editMsg"><input type="number" maxlength="11" v-model="orderInfo.phone" placeholder="请输入电话" placeholder-class="placeholder" /></view>
						</view>
					</view>
					<view class="pdlr4 radius10" v-show="curr==2">
						
						<view class="flexRowBetween  pdtb15 borderB1"  v-if="addressData.name"
						@click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
							<view class="fs14 color6">
								<view class="flex mgt5">
									<view class="mgr15 color2">{{addressData.name}}</view>
									<view>{{addressData.phone}}</view>
								</view>
								<view class="mgt5">{{addressData.city+addressData.detail}}</view>
							</view>
							<view class="flexEnd" style="width: 20%;">
								<image class="arrowR" src="../../static/images/addressl-icon2.png" mode=""></image>
							</view>
						</view>
						<view class="flexRowBetween  pdtb15 borderB1" v-else
						@click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
							<view class="fs14 color6 flex">
								<view style="width: 36rpx;height: 36rpx;margin-right: 10rpx;"><image style="" src="../../static/images/shoppimg-icon2.png" mode=""></image></view>
								<view class="red">选择收货地址</view>
							</view>
							<view class="flexEnd" style="width: 20%;">
								<image class="arrowR" src="../../static/images/addressl-icon2.png" mode=""></image>
							</view>
						</view>
						<view class="pdt15 red">5公里范围内均可配送</view>
					</view>
				</view>
			</view>
			<view class="proRow mgt15" v-for="(item,index) in mainData" :key="index">
				<view class="item flexRowBetween borderB1" >
					<view class="pic"><image :src="item.product&&item.product.mainImg&&item.product.mainImg[0]?item.product.mainImg[0].url:''" mode=""></image></view>
					<view class="infor">
						<view class="tit">{{item.product&&item.product.title?item.product.title:''}}</view>
						<view class="flexRowBetween B-price">
							<view class="price ftw">{{item.product&&item.product.price?item.product.price:''}}</view>
							<view class="flexEnd">
								<view class="numBox flex">
									<view class="btn" @click="counter(index,'-')">-</view>
									<view class="num">{{item.count}}</view>
									<view class="btn pubBj white add"  @click="counter(index,'+')">+</view>
								</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		
		<view class="xqbotomBar">
			<view class="flex mgl15 fs13">总计：<view class="price fs14">{{totalPrice}}<span v-if="deliver>0&&curr==2" class="fs10">(含配送费{{deliver}}元)</span></view></view>
			<button class="payBtn" v-if="curr==2" open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)">立即支付</button>
			<view class="payBtn" v-if="curr==1" @click="payShowShow">立即支付</view>
		</view>
		
		<!-- 弹框 -->
		<view class="black-bj" v-show="is_show"></view>
		
		<view class="xieyiShow payShow whiteBj radius10" v-show="is_payShow">
			<view class="center pdtb15 color6">订单确认</view>
			<view class="">
				<view class="borderB1"></view>
				<view class="borderB1 pdlr4 pdt15 pdb15 fs13">
					<view class="mgb10">门店名称：{{shopData.name}}</view>
					<view class="mgb10">门店电话：{{shopData.phone}}</view>
					<view class="">门店地址：{{shopData.address}}</view>
				</view>
				<view class="center flexRowBetween">
					<view style="width: 50%; height: 90rpx;line-height: 90rpx;color:#58a3ff ;box-sizing: border-box;border-right: 1px solid #eee;" 
					@click="payShowShowClose">重新输入</view>
					<button style="width: 50%;height: 90rpx;line-height: 90rpx;" class="pubColor" 
					open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)">确认</button>
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
				Utils:this.$Utils,
				showView: false,
				score:'',
				wx_info:{},
				is_show:false,
				curr:1,
				count:1,
				
				is_payShow:false,
				shopData:{},
				addressData:{},
				pay:{},
				totalPrice:0,
				mainData:[],
				nearShopData:[],
				orderInfo:{
					name:'',
					phone:''
				},
				deliver:0
			}
		},
		
		onLoad(options) {
			const self = this;
			//self.$Utils.loadAll(['getUserData'], self);
			const callback = (res) => {
				self.today = new Date().getTime()+uni.getStorageSync('user_info').thirdApp.pickup_time*86400000;
				self.mainData = uni.getStorageSync('payPro');
				self.countTotalPrice()
			};
			self.$Token.getProjectToken(callback, {
				refreshToken: true
			})
			
		},
		
		onShow() {
			const self = this;
			if(self.curr==1){
				if(uni.getStorageSync('choosedShopData')){
					self.shopData = uni.getStorageSync('choosedShopData')
				}else{
					self.getDefalutShop()
				};
			}else{
				if(uni.getStorageSync('choosedAddressData')){
					self.addressData = uni.getStorageSync('choosedAddressData')
					
					self.getNearShop()
				}else{
					self.getAddressData()
				};
			}
		},
		
		methods: {
			
			//获取默认门店
			getDefalutShop() {
				const self = this;		
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				
				postData.getAfter = {
					shop:{
						tableName:'UserInfo',
						middleKey:'passage1',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'='
					}
				}
				const callback = (res) => {
					if (res.info.data.length > 0) {
						if(res.info.data[0].shop[0]){
							self.shopData= res.info.data[0].shop[0]
						}
					}
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			//获取默认收货地址
			getAddressData() {
				const self = this;		
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					isdefault:1
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.addressData = res.info.data[0];
					}
				};
				self.$apis.addressGet(postData, callback);
			},
			
			//获取最近门店
			getNearShop() {
				const self = this;
				self.nearShopData = [];
				const postData = {};
				console.log(222)
				postData.searchItem = {
					thirdapp_id: 2,
					user_type:1
				};
				postData.order = {
					distance:'asc',
					longitude:self.addressData.longitude,
					latitude:self.addressData.latitude,
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.nearShopData.push.apply(self.nearShopData, res.info.data);
						for (var i = 0; i < self.nearShopData.length; i++) {
							self.nearShopData[i].distance =
								self.$Utils.distance(self.addressData.latitude, self.addressData.longitude, self.nearShopData[i].latitude, self.nearShopData[i].longitude)
						}
						console.log('self.nearShopData', self.nearShopData)
						if(self.nearShopData[0].distance<parseFloat(uni.getStorageSync('user_info').thirdApp.distance)){
							self.shop_no = self.nearShopData[0].user_no
							console.log('self.shop_no',self.shop_no)
						}else{
							uni.showModal({
							    title: '提示',
							    content: '该地址超出送货范围，请重新选择地址',
								confirmColor:'#FC73AA',
								showCancel:false,
							    success: function (res) {
							        if (res.confirm) {
										
										self.addressData = {}
							        } else if (res.cancel) {
							           
							        }
							    }
							});	
						}
					}

				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			currChange(curr){
				const self = this;	
				if(curr!=self.curr){
					self.curr = curr
					if(self.curr==1){
						if(uni.getStorageSync('choosedShopData')){
							self.shopData = uni.getStorageSync('choosedShopData')
						}else{
							self.getDefalutShop()
						};
					}else{
						if(uni.getStorageSync('choosedAddressData')){
							self.addressData = uni.getStorageSync('choosedAddressData')
							
							self.getNearShop()
						}else{
							self.getAddressData()
						};
					};
					self.countTotalPrice()
				}
			},
			
			counter(index, type) {
				const self = this;
				if (type == '+') {
					self.mainData[index].count++;
				} else {
					if (self.mainData[index].count > 1) {
						self.mainData[index].count--;
					}
				};
				self.countTotalPrice();
			},
			
			countTotalPrice() {
				const self = this;
				self.totalPrice = 0;
				
				for (var i = 0; i < self.mainData.length; i++) {
					self.totalPrice += self.mainData[i].product.price*self.mainData[i].count
				}
				
				if(self.curr==2&&self.totalPrice<parseFloat(uni.getStorageSync('user_info').thirdApp.delivery_free)){
					self.deliver = parseFloat(uni.getStorageSync('user_info').thirdApp.delivery_fee);
					self.totalPrice = parseFloat(self.totalPrice) + parseFloat(uni.getStorageSync('user_info').thirdApp.delivery_fee)
				}
				self.totalPrice = parseFloat(self.totalPrice).toFixed(2)
				//console.log('wxPay',wxPay)
				if (self.totalPrice > 0) {
					self.pay.wxPay = {
						price: self.totalPrice,
					};
				} else {
					  delete self.pay.wxPay;	 
				};
				console.log(self.pay)
			},
			
			submit(){
				const self = this;
				uni.setStorageSync('canClick', false);
				self.is_payShow = false;
				self.is_show = false;
				if(self.curr==1){
					if(self.orderInfo.name==''||self.orderInfo.phone==''){
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('请填写收货人信息','none')
						return
					}
				}else{
					if(JSON.stringify(self.addressData) == '{}'){
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('请选择收货地址','none')
						return
					}
					if(self.shop==''){
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('门店匹配错误','none')
						return
					}
				};
				var data = {
					transport_type:self.curr,
					snap_address:self.addressData,
				};
				if(self.curr==1){
					data.pickup_time = self.today/1000;
					data.name = self.orderInfo.name;
					data.phone = self.orderInfo.phone;
					data.shop_no = self.shopData.user_no
				};
				if(self.curr==2&&self.deliver>0){
					data.delivery_fee = self.deliver
					data.shop_no = self.shop_no
				};
				var orderList = []
				for (var i = 0; i < self.mainData.length; i++) {
					orderList.push({product_id:self.mainData[i].product_id,count:self.mainData[i].count,type:self.mainData[i].type,data:data})
				}
				const callback = (user, res) => {
					self.addOrder(orderList)
				};
				self.$Utils.getAuthSetting(callback);
			},
			
			addOrder(orderList) {
				const self = this;	
				
				/* if(self.orderId){
					self.goPay()
					return
				}; */
				const postData = {}; 
				postData.orderList = self.$Utils.cloneForm(orderList);
				postData.data = {
					transport_type:self.curr,
					level:1,
					snap_address:self.addressData,
					price:self.totalPrice
				};
				postData.parent = 1;
				postData.tokenFuncName = 'getProjectToken';
				if(self.curr==1){
					postData.data.name = self.orderInfo.name;
					postData.data.phone = self.orderInfo.phone;
					postData.data.shop_no = self.shopData.user_no;
					postData.data.pickup_time = self.today
				};
				if(self.curr==2&&self.deliver>0){
					postData.data.delivery_fee = self.deliver
					postData.data.shop_no = self.shop_no
				};
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res && res.solely_code == 100000) {
						self.orderId = res.info.id;
						var array = self.$Utils.getStorageArray('cartData');
						for (var i = 0; i < orderList.length; i++) {
							for (var j = 0; j < array.length; j++) {
								if(orderList[i].product_id == array[j].id){
									self.$Utils.delStorageArray('cartData', orderList[i], 'id');
								}
							}
						};
						self.goPay()
					} else {		
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};		
				};
				self.$apis.addOrder(postData, callback);
			},
			
			goPay() {
				const self = this;	
				const postData = self.$Utils.cloneForm(self.pay);	
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
					
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									uni.showToast({
										title: '支付成功',
										duration: 1000,
										success: function() {
											
										}
									});
									if(self.curr==1){
										setTimeout(function() {
											self.$Router.redirectTo({route:{path:'/pages/orderPayOk/orderPayOk?id='+self.orderId}})
										}, 1000);
									}else{
										setTimeout(function() {
											self.$Router.redirectTo({route:{path:'/pages/userOrder/userOrder'}})
										}, 1000);
									}
									
								} else {
									uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '支付失败',
										duration: 2000
									});
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							
							uni.showToast({
								title: '支付成功',
								duration: 1000,
								success: function() {
									
								}
							});
							if(self.curr==1){
								setTimeout(function() {
									self.$Router.redirectTo({route:{path:'/pages/orderPayOk/orderPayOk?id='+self.orderId}})
								}, 1000);
							}else{
								setTimeout(function() {
									self.$Router.redirectTo({route:{path:'/pages/userOrder/userOrder'}})
								}, 1000);
							}
						};
					} else {
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};
				};
				self.$apis.pay(postData, callback);
			},
			
			payShowShow(){
				const self = this;
				if(JSON.stringify(self.shopData) == '{}'){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请选择自提门店','none')
					return
				};
				if(self.orderInfo.name==''||self.orderInfo.phone==''){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请填写收货人信息','none')
					return
				};
				self.is_show = !self.is_show
				self.is_payShow = !self.is_payShow
			},
			
			
			
			payShowShowClose(){
				const self = this;
				self.is_show = false
				self.is_payShow = false
			}
		}
	};
</script>

<style>
	@import "../../assets/style/detail.css";
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;}
	
	.editMsg{width: 45%;height: 60rpx;padding: 0 10rpx;box-sizing: border-box;border: 1px solid #eee;box-sizing: border-box;text-align: center;}
	.editMsg input{width: 100%;height: 60rpx;line-height: 60rpx;box-sizing: border-box;text-align: center;font-size: 26rpx;}
	
	.seltTake .btn{width: 50%;height: 80rpx;line-height: 80rpx;background: #f8f8f8;}
	.seltTake .btn.on{background: #fff;}
	.seltTake .btn:last-child.on{border-radius: 40rpx 0 0 0;}
	
	.proRow .item .pic{width: 140rpx; height: 140rpx;}
	.proRow .item .infor{width: 75%; height: 140rpx;}
	
	
	.xieyiShow{width: 90%;min-height: 300rpx;position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);z-index: 50;box-sizing: border-box;}
	.xieyiShow .xqInfor .cont{height: 400rpx;padding-bottom: 80rpx;}
	button{
		background: none;
		line-height: 1.5;
		margin-left: 0;
		margin-right: 0;
		border-radius: 0;
		font-size:14px
	}
	button::after{
		border: none;
	}
	.button-hover {
		color: #000000;
		background: none;
	}
	
</style>
