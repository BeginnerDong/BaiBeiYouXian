<template>
	<view>
		<view class="fs13 pdlr4">
			<view class="TopItem flex pdtb15 borderB1">
				<view class="flex">
					<image class="icon" src="../../static/images/contact-usl-icon.png" mode=""></image>
				</view>
				<view class="text">{{mainData.description}}</view>
			</view>
			<view class="TopItem flex pdtb15">
				<view class="flex">
					<image class="icon" src="../../static/images/contact-usl-icon1.png" mode=""></image>
				</view>
				<view class="text">{{mainData.small_title}}</view>
			</view>
		</view>
		<view class="f5H10"></view>
		<view class="mglr4 pdtb15">
			<view class="fs15 ftw">公司简介</view>
			<view class="xqInfor pdt10">
				<view class="cont fs13 color6">
					<view class="content ql-editor" style="padding:0;"
					v-html="mainData.content">
					</view>
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
				mainData:{}
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2
				};
				postData.getBefore = {
					article:{
						tableName:'Label',
						middleKey:'menu_id',
						key:'id',
						searchItem:{
							title: ['in', ['联系我们']],
						},
						condition:'in'
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						//const type = new RegExp('<img');
						
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
						//self.mainData.content = self.mainData.content.replace(type, `<image`);
						
					};
					console.log(self.mainData.content)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},

		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/detail.css";
	page{padding-bottom: 60rpx;}
	
	.TopItem .icon{width: 30rpx;height: 30rpx;margin-right: 20rpx;}
	.TopItem .text{width: 90%;}
	
</style>
