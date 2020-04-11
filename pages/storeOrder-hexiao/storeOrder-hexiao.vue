<template>
	<view>
		
		<view class="mglr4 pdt15">
			<view class="proRow">
				<view class="item">
					<view class="fs12 flexRowBetween mgb10">
						<view class="color9">交易时间：2020-01-18</view>
						<view class="red">待核销</view>
					</view>
					<view class="flexRowBetween pdb15 borderB1">
						<view class="pic">
							<image src="../../static/images/shopping-img.png" mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow2 fs13">墨西哥牛油果8枚单果200g左右</view>
							<view class="B-price flexRowBetween">
								<view class="price">138</view>
								<view class="fs13">×1</view>
							</view>
						</view>
					</view>
					<view class="pdt15 flexRowBetween fs13">
						<view class="color6">客户信息：</view>
						<view class="flexEnd">
							<view>张单</view>
							<view class="mgl10">1567894567</view>
						</view>
					</view>
				</view>
			</view>
			
		</view>
		<view class="submitbtn" style="margin-top:160rpx;">
			<button class="btn" type="button">确认核销</button>
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
				is_show:false
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
		}
	};
</script>

<style>
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;}
	
	.proRow .item .pic{width: 160rpx;height: 160rpx;}
	.proRow .item .infor{width: 72%;height: 160rpx;}
	
	textarea{color: #222;padding: 30rpx;}
</style>
