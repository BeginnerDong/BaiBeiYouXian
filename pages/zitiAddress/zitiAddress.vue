<template>
	<view>
		<view class="myaddress-lis flexRowBetween" v-for="(item,index) in mainData" :key="index">
			<view class="ll">
				<view class="flex title">
					<view class="name flex"><image class="icon" src="../../static/images/the-pointl-icon.png" mode=""></image>{{item.name}}</view>
					<view class="mgl15 numb color6 fs13">{{item.phone}}</view>
				</view>
				
				<view class="adrs">{{item.address}}</view>
			</view>
			<view class="rr flexEnd">
				<view class="seltBox" @click="setDefault(index)">
					<image class="icon" :src="defaultNo!=''&&defaultNo == item.passage1?'../../static/images/shoppimg-icon.png':'../../static/images/shoppimg-icon1.png'" mode=""></image>
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
				
				mainData:[],
				defaultNo:''
			}
		},

		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.defaultNo = uni.getStorageSync('user_info').info.passage1;
			self.$Utils.loadAll(['getMainData'], self);
		},

		
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		

		methods: {
			
			setDefault(index) {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.data = {
					passage1:self.mainData[index].user_no
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.defaultNo = self.mainData[index].passage1
						uni.setStorageSync('choosedShopData', self.mainData[index]);
						console.log('choosedIndex', self.choosedIndex);
						uni.navigateBack({
							delta: 1
						})
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.userInfoUpdate(postData, callback);
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
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					user_type:1,
					thirdapp_id:2
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data)
					};
					self.$Utils.finishFunc('getMainData');	
				};
				self.$apis.userInfoGet(postData, callback);
			},
		}
	}
</script>

<style>
	/* @import "../../assets/style/seltAlert.css"; */
	/* page{ background: #f5f5f5;} */
	.myaddress-lis{padding:30rpx 4%;border-bottom: 20rpx solid #F5F5F5;background: #fff; }
	.myaddress-lis .ll{ width: 85%;}
	/* .myaddress-lis .name{ font-size: 28rpx; color: #222;} */
	.myaddress-lis .ll .title{flex-wrap: wrap;}
	.myaddress-lis .ll .icon{ width: 26rpx; height: 26rpx; display: inline-block; margin-right: 10rpx;}
	.myaddress-lis .adrs{ font-size: 26rpx;color: #666; line-height: 40rpx;padding-top: 16rpx;}
	
	.myaddress-lis .rr{ width: 15%;}
	.seltBox{ width:40rpx; height: 40rpx; display:block;}
</style>
