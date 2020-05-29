<template>
	<view>
		<!-- 搜索 -->
		<view class="seachbox flexRowBetween whiteBj borderB1"  style="padding-bottom: 30rpx;"> 
			<view class="flex rr" style="width: 85%;">
				<button class="seachBtn" type="button">
					<image src="../../static/images/home-icon.png" mode=""></image>
				</button>
				<view class="input">
					<input type="text" name="" v-model="keywords" placeholder="搜索商品" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="fs15 pubColor" @click="search">搜索</view>
		</view>
		<view class="mglr4 flexRowBetween productList mgt15">
			<view class="item radius10 pr" v-for="(item,index) in mainData" :key="index" >
				<view class="fixState" style="background-color: #fb7445;" v-if="item.is_Buying&&!item.is_noStock">抢购中</view>
				<view class="fixState" style="background-color: #0d9f44;" v-if="item.is_notBuying&&!item.is_noStock">预售中</view>
				<view class="fixState" style="background-color: #9b9b9b;" v-if="item.is_noStock">已售罄</view>
		
				<view class="pic" :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail?id='+$event.currentTarget.dataset.id}})">
					<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
				</view>
				<view class="infor">
					<view class="tit avoidOverflow fs15">{{item.title}}</view>
					<view class="fs11 color6">销量：{{item.sale_count}}</view>
					<view class="fs11 time color6">提货时间：{{Utils.timeto(today,'md')}}</view>
					<view class=" pdt10 flexRowBetween">
						<view class="flex">
							<view class="price fs16 ftw">{{item.price}}</view>
							<view class="fs10 color6" v-if="item.is_notBuying&&!item.is_noStock">预售时间：{{Utils.timeto(item.end_time,'md')}}</view>
						</view>
						<view class="carIcon" @click="addCar(index)"><image src="../../static/images/icon-01.png" mode=""></image></view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 无数据 -->
		<view class="nodata" v-if="mainData.length==0"><image src="../../static/images/nodata.png" mode=""></image></view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				searchItem:{
					thirdapp_id:2,
				},
				keywords:'',
				mainData:[],
				today:0
			}
		},
		
		onLoad(options) {
			const self = this;
			self.today = new Date().getTime()+uni.getStorageSync('user_info').thirdApp.pickup_time*86400000;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			addCar(index) {
				const self = this;
				var array = self.$Utils.getStorageArray('cartData');
				for (var i = 0; i < array.length; i++) {
					if (array[i].id == self.mainData[index].id) {
						var target = array[i]
					}
				}
				if (target) {
					target.count = target.count + 1
				} else {
					var target = self.mainData[index];
					target.count = 1;
					target.isSelect = true;
				}
				self.$Utils.showToast('加入成功', 'none');
				self.$Utils.setStorageArray('cartData', target, 'id', 999);
			},
			
			search(){
				const self = this;
				if(self.keywords!=''){
					self.getMainData(true)
				}else{
					self.$Utils.showToast('请输入','none');
				}
			},
			
			getMainData(isNew) {
				const self = this;
				var now = new Date().getTime();
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						pagesize: 10,
						is_page: true,
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem)
				postData.searchItem.start_time = ['<', now]
				postData.searchItem.title = ['like',['%'+self.keywords+'%']]
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							if (self.mainData[i].stock == 0 || self.mainData[i].sellout == 1) {
								self.mainData[i].is_noStock = true
							};
							if (parseInt(self.mainData[i].end_time) > now) {
								self.mainData[i].is_notBuying = true
							} else {
								self.mainData[i].is_Buying = true
							}
						}
					} else {
			
					};
					self.$Utils.finishFunc('getMainData');
			
				};
				self.$apis.productGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/seach.css";
	@import "../../assets/style/productList.css";
	
	page{padding-bottom: 110rpx;background: #F5F5F5;}
</style>

