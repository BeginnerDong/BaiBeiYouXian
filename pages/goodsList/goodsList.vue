<template>
	<view>
		
		<view class="">
			
			<view class="mglr4 flexRowBetween productList mgt15">
				<view class="item radius10 pr" v-for="(item,index) in mainData" :key="index" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail?id='+$event.currentTarget.dataset.id}})">
					<view class="fixState" style="background-color: #fb7445;" v-if="item.is_Buying&&!item.is_noStock">抢购中</view>
					<view class="fixState" style="background-color: #0d9f44;" v-if="item.is_notBuying&&!item.is_noStock">预售中</view>
					<view class="fixState" style="background-color: #9b9b9b;" v-if="item.is_noStock">已售罄</view>
			
					<view class="pic">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="infor">
						<view class="tit avoidOverflow fs15">{{item.title}}</view>
						<view class="fs11 color6">销量：525</view>
						<view class="fs11 time color6">提货时间：{{Utils.timeto(today,'md')}}</view>
						<view class=" pdt10 flexRowBetween">
							<view class="flex">
								<view class="price fs16 ftw">{{item.price}}</view>
								<view class="fs10 color6" v-if="item.is_notBuying&&!item.is_noStock">预售时间：{{Utils.timeto(item.end_time,'md')}}</view>
							</view>
							<view class="carIcon"><image src="../../static/images/icon-01.png" mode=""></image></view>
						</view>
					</view>
				</view>
			</view>
			
			<!-- 无数据 -->
			<view class="nodata" v-if="mainData.length==0"><image src="../../static/images/nodata.png" mode=""></image></view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				is_show: false,
				wx_info:{},
				labelData:[],
				searchItem:{
					thirdapp_id:2,
				},
				today:0,
				mainData:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.today = new Date().getTime()+uni.getStorageSync('user_info').thirdApp.pickup_time*86400000;
			
			self.searchItem.category_id = options.id
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData','getLabelData'], self);
		},
		
		methods: {
			
			change(id){
				const self = this;
				if(searchItem.category_id!=id){
					self.searchItem.category_id = id;
					self.getMainData(true)
				}
			},
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type:3
				};
				postData.order = {
					listorder: 'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData.push.apply(self.labelData, res.info.data);
					}
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.labelGet(postData, callback);
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
				postData.order = {
					sale_count: 'desc'
				};
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
	@import "../../assets/style/productList.css";
	
	page{background-color: #F5F5F5;}
	
</style>
