<template>
	<view>
		
		<view class="">
			<view class="">
				<scroll-view class="scollNav color6 boxShaow" scroll-x >
					<view class="tt" v-for="(item,index) in navData" :key="index">{{item}}</view>
				</scroll-view>
			</view>
			<view class="mglr4 flexRowBetween productList mgt15">
				<view class="item radius10 pr" v-for="(item,index) in productData" :key="index" @click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail'}})">
					<view class="fixState" style="background-color: #0d9f44;">预售中</view>
					<view class="fixState" style="background-color: #9b9b9b;">已售罄</view>
					<view class="fixState" style="background-color: #fb7445;">抢购中</view>
					
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
		</view>
		
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
				navData:['全部','日用','水果','蔬菜','粮油','生鲜','蛋类'],
				productData:[{},{},{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
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
	@import "../../assets/style/productList.css";
	
	page{background-color: #F5F5F5;}
	.scollNav{white-space: nowrap;}
	.scollNav .tt{display: inline-block;margin:0 30rpx;height: 80rpx;line-height: 80rpx;}
	.scollNav .tt.on{color: #FB7445;}
</style>
