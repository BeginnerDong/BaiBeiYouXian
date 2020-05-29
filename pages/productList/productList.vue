<template>
	<view>
		
		<view class="">
			<view class="whiteBj">
				<scroll-view class="scollNav color6 boxShaow" scroll-x >
					<view @click="change(item.id)" class="tt" :class="searchItem.category_id==item.id?'on':''" 
					v-for="(item,index) in labelData" :key="index">{{item.title}}</view>
				</scroll-view>
			</view>
			
			<view class="indHome flex whiteBj pdt15 fs13">
				<view class="item mgb15" v-for="(item,index) in labelTwoData" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/goodsList/goodsList?id='+$event.currentTarget.dataset.id}})">
					<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></image>
					<view class="tit">{{item.title}}</view>
				</view>
			</view>
			
			<view class="mglr4 flexRowBetween productList mgt15">
				<view class="item radius10 pr" v-for="(item,index) in mainData" :key="index">
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
				productData:[{},{},{},{},{}],
				labelData:[],
				labelTwoData:[],
				searchItem:{
					thirdapp_id:2,
				},
				searchItemTwo:{
					thirdapp_id:2,
				},
				today:0,
				mainData:[],
				typeData:[
					{iconUrl:'../../static/images/icon-03.png',title:'苹果'},
					{iconUrl:'../../static/images/icon-04.png',title:'香蕉'},
					{iconUrl:'../../static/images/icon-05.png',title:'樱桃'},
					{iconUrl:'../../static/images/icon-06.png',title:'石榴'},
					{iconUrl:'../../static/images/icon-07.png',title:'火龙果'},
					{iconUrl:'../../static/images/icon-08.png',title:'迷糊桃'},
					{iconUrl:'../../static/images/icon-09.png',title:'菠萝'},
					{iconUrl:'../../static/images/icon-10.png',title:'水蜜桃'}
				],
				idArray:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.today = new Date().getTime()+uni.getStorageSync('user_info').thirdApp.pickup_time*86400000;
			
			self.searchItem.category_id = options.id;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getLabelData'], self);
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
			
			change(id){
				const self = this;
				if(self.searchItem.category_id!=id){
					self.searchItem.category_id = id;
					self.labelTwoData = [];
					self.mainData = [];
					self.getLabelTwoData()
				}
			},
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type:3,
					parentid:0
				};
				postData.order = {
					listorder: 'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData.push.apply(self.labelData, res.info.data);
						self.getLabelTwoData()
					}else{
						self.$Utils.finishFunc('getLabelData');
					}
					
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getLabelTwoData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type:3,
					parentid:self.searchItem.category_id
				};
				postData.order = {
					listorder: 'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelTwoData.push.apply(self.labelTwoData, res.info.data);
						for (var i = 0; i < self.labelTwoData.length; i++) {
							self.idArray.push(self.labelTwoData[i].id)
						}
						self.searchItemTwo.category_id = ['in',self.idArray]
						self.getMainData(true)
					}else{
						self.$Utils.finishFunc('getLabelData');
					}
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
				postData.searchItem = self.$Utils.cloneForm(self.searchItemTwo)
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
					self.$Utils.finishFunc('getLabelData');
			
				};
				self.$apis.productGet(postData, callback);
			},
			
			
		}
	};
</script>

<style>
	@import "../../assets/style/productList.css";
	
	page{background-color: #F5F5F5;}
	.scollNav{white-space: nowrap;}
	.scollNav .tt{display: inline-block;margin:0 30rpx;height: 80rpx;line-height: 80rpx;}
	.scollNav .tt.on{color: #FB7445;position: relative;}
	.scollNav .tt.on::after{content: ""; width: 40rpx; height: 6rpx;background: #fb7445; position: absolute; bottom: 0;left: 50%;transform: translateX(-50%);}
	
	
	/* 分类导航 */
	.indHome {
		flex-wrap: wrap;
	}
	
	.indHome .item {
		width: 25%;
		text-align: center;
		color: #222;
		display: flex;
		flex-direction: column;
		line-height: 36rpx;
	}
	
	.indHome .item image {
		width: 68rpx;
		height: 68rpx;
		margin: 0 auto 20rpx auto;
	}
	
</style>
