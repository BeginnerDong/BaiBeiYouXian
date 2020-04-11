<template>
	<view>
		
		<view class="homeHead">
			<view class="bj"><image src="../../static/images/successfull-img.png" mode=""></image></view>
			<view class="Hcont">
				<view class="seachbox">
					<view class="flex rr" style="width: 100%;" @click="Router.navigateTo({route:{path:'/pages/seach/seach'}})">
						<button class="seachBtn" type="button"><image src="../../static/images/home-icon.png" mode=""></image></button>
						<view class="input">
							<input type="text" name="" value="" placeholder="搜索活动" placeholder-class="placeholder" />
						</view>
					</view>
				</view>
				<view class="banner-box pdlr4">
					<view class="banner">
						<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-active-color="#fb7445">
							<block v-for="(item,index) in labelData" :key="index">
								<swiper-item class="swiper-item">
									<image :src="item" class="slide-image" />
								</swiper-item>
							</block>
						</swiper>
					</view>
				</view>
			</view>
		</view>
		
		<view class="indHome flexRowBetween whiteBj pdt20 fs13">
			<view class="item"  @click="Router.navigateTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon1.png"></image>
				<view class="tit">日用</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon2.png"></image>
				<view class="tit">水果</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon3.png"></image>
				<view class="tit">蔬菜</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon4.png"></image>
				<view class="tit">粮油</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon5.png"></image>
				<view class="tit">生鲜</view>
			</view>
			<view class="item"  @click="Router.navigateTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon1.png"></image>
				<view class="tit">日用</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon2.png"></image>
				<view class="tit">水果</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon3.png"></image>
				<view class="tit">蔬菜</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon4.png"></image>
				<view class="tit">粮油</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon5.png"></image>
				<view class="tit">生鲜</view>
			</view>
		</view>
		
		<view class="R-fixIcon" @click="call()"><image src="../../static/images/home-icon6.png" mode=""></image></view>
		
		
		<view class=" pdlr4 whiteBj pdb15">
			<view class="Big-title fs15 pdt5 ftw">推荐课程</view>
		</view>
		
		<view class="mglr4 flexRowBetween productList mgt15">
			<view class="item radius10 pr" v-for="(item,index) in productData" :key="index" @click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail'}})">
				<view class="fixState" style="background-color: #fb7445;">抢购中</view>
				<view class="fixState" style="background-color: #0d9f44;">预售中</view>
				<view class="fixState" style="background-color: #9b9b9b;">已售罄</view>
				
				<view class="pic"><image src="../../static/images/home-img.png" mode=""></image></view>
				<view class="infor">
					<view class="tit avoidOverflow fs15">墨西哥牛油果8枚单果200g左右</view>
					<view class="fs10 color6">提货时间：3月20日</view>
					<view class=" pdt10 flexRowBetween">
						<view class="price fs16 ftw">88</view>
						<view class="fs10 color6">预售时间：3月20日</view>
					</view>
				</view>
			</view>
		</view>
		<!-- 无数据 -->
		<view class="nodata"><image src="../../static/images/nodata.png" mode=""></image></view>
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1-a.png" />
				</view>
				<view class="text this-text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/car/car'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">购物车</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
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
				Router:this.$Router,
				is_show: false,
				wx_info:{},
				is_show:false,
				labelData: [
					"../../static/images/home-banner.png",
					"../../static/images/home-banner.png",
					"../../static/images/home-banner.png"
				],
				productData:[{},{},{},{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			call(){
				uni.makePhoneCall({
					phoneNumber:'15802977845'
				});
			},
			
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/seach.css";
	@import "../../assets/style/productList.css";
	
	page{padding-bottom: 140rpx;background-color: #F5F5F5;}	
	
	.homeHead{/* height: 470rpx; */position: relative;    z-index: 1;}
	.homeHead .bj{width: 100%;height:270rpx;position: absolute;left: 0;top: 0;right: 0;    z-index: -1;}
	
	.swiper-box {height: 300rpx;box-sizing: border-box;overflow: hidden;border-radius: 10rpx;}
	.swiper-box swiper-item{width: 100%;box-sizing: border-box;overflow: hidden;}
	.swiper-box swiper-item image{width: 100%;height: 100%;box-sizing: border-box;}
	
	.R-fixIcon{width: 90rpx;height: 90rpx;position: fixed;bottom:30%;right: 30rpx;z-index: 66;}
	.R-fixIcon image{width: 100%;height: 100%;}
	
	/* 分类导航 */
	.indHome{ flex-wrap:wrap;}
	.indHome .item{width: 20%;text-align: center;color: #222;display: flex; flex-direction: column;margin-bottom: 40rpx;line-height: 36rpx; }
	.indHome .item image{width:90rpx;height:90rpx; margin: 0 auto 20rpx auto;}
	
	.Big-title{position: relative;padding-left: 10rpx;z-index: 1;}
	.Big-title::before{content: '';width: 30rpx;height: 10rpx;background-image: linear-gradient(to bottom, #fff7c2 , #ffd562);position: absolute;bottom: 4rpx;left: 0; z-index: -1;}
	
</style>
