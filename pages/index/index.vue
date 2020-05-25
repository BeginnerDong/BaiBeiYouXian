<template>
	<view>

		<view class="homeHead">
			<view class="bj">
				<image src="../../static/images/successfull-img.png" mode=""></image>
			</view>
			<view class="Hcont">
				<view class="seachbox">
					<view class="flex rr" style="width: 100%;" @click="Router.navigateTo({route:{path:'/pages/seach/seach'}})">
						<button class="seachBtn" type="button">
							<image src="../../static/images/home-icon.png" mode=""></image>
						</button>
						<view class="input">
							<input type="text" disabled="true" placeholder="搜索商品" placeholder-class="placeholder" />
						</view>
					</view>
				</view>
				<view class="banner-box pdlr4">
					<view class="banner">
						<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000"
						 indicator-active-color="#fb7445">
							<block v-for="(item,index) in sliderData.mainImg" :key="index">
								<swiper-item class="swiper-item">
									<image :src="item.url" class="slide-image" />
								</swiper-item>
							</block>
						</swiper>
					</view>
				</view>
			</view>
		</view>

		<view class="indHome flexRowBetween whiteBj pdt20 fs13">
			<view class="item" v-for="(item,index) in labelData" :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/productList/productList?id='+$event.currentTarget.dataset.id}})">
				<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></image>
				<view class="tit">{{item.title}}</view>
			</view>
			
		</view>

		<button class="R-fixIcon" open-type="contact">
			<image src="../../static/images/home-icon6.png" mode=""></image>
		</button>


		<view class=" pdlr4 whiteBj pdb15">
			<view class="Big-title fs15 pdt5 ftw">热销商品</view>
		</view>

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
		<view class="nodata" v-if="mainData.length==0">
			<image src="../../static/images/nodata.png" mode=""></image>
		</view>

		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1-a.png" />
				</view>
				<view class="text this-text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/car/car'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">购物车</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar3.png" />
				</view>
				<view class="text">我的</view>
			</view>
		</view>
		<!--底部tab键 end-->

	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				Utils:this.$Utils,
				is_show: false,
				wx_info: {},
				
				sliderData: [
					
				],
				productData: [{}, {}, {}, {}, {}, {}],
				mainData:[],
				labelData:[],
				today:0
			}
		},


		onLoad(options) {
			const self = this;
			
			const callback = (res) => {
				self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
				self.$Utils.loadAll(['getMainData','getLabelData','getSliderData','getUserInfoData'], self);
				self.today = new Date().getTime()+uni.getStorageSync('user_info').thirdApp.pickup_time*86400000;
				
			};
			self.$Token.getProjectToken(callback, {
				refreshToken: true
			})
			
			
			
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
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0]
					}
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getSliderData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title:"首页轮播"
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.sliderData = res.info.data[0]
					}
					self.$Utils.finishFunc('getSliderData');
				};
				self.$apis.labelGet(postData, callback);
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
				postData.searchItem = {
					thirdapp_id: 2,
					start_time: ['<', now],
				};

				postData.order = {
					listorder: 'desc'
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
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/seach.css";
	@import "../../assets/style/productList.css";

	page {
		padding-bottom: 140rpx;
		background-color: #F5F5F5;
	}

	.homeHead {
		/* height: 470rpx; */
		position: relative;
		z-index: 1;
	}

	.homeHead .bj {
		width: 100%;
		height: 270rpx;
		position: absolute;
		left: 0;
		top: 0;
		right: 0;
		z-index: -1;
	}

	.swiper-box {
		height: 300rpx;
		box-sizing: border-box;
		overflow: hidden;
		border-radius: 10rpx;
	}

	.swiper-box swiper-item {
		width: 100%;
		box-sizing: border-box;
		overflow: hidden;
	}

	.swiper-box swiper-item image {
		width: 100%;
		height: 100%;
		box-sizing: border-box;
	}

	.R-fixIcon {
		width: 90rpx;
		height: 90rpx;
		position: fixed;
		bottom: 30%;
		right: 30rpx;
		z-index: 66;
	}

	.R-fixIcon image {
		width: 100%;
		height: 100%;
	}

	/* 分类导航 */
	.indHome {
		flex-wrap: wrap;
	}

	.indHome .item {
		width: 20%;
		text-align: center;
		color: #222;
		display: flex;
		flex-direction: column;
		margin-bottom: 40rpx;
		line-height: 36rpx;
	}

	.indHome .item image {
		width: 90rpx;
		height: 90rpx;
		margin: 0 auto 20rpx auto;
	}

	.Big-title {
		position: relative;
		padding-left: 10rpx;
		z-index: 1;
	}

	.Big-title::before {
		content: '';
		width: 30rpx;
		height: 10rpx;
		background-image: linear-gradient(to bottom, #fff7c2, #ffd562);
		position: absolute;
		bottom: 4rpx;
		left: 0;
		z-index: -1;
	}
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
