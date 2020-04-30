<template>
	<view v-if="showAll">
		
		<view class="loginBj pr">
			<image src="../../static/images/the-login-img.png" mode=""></image>
		</view>
		
		<view class="loginCont">
			<view class="item flex mgb20">
				<view class="input">
					<input type="text"  v-model="submitData.login_name" placeholder="账号" placeholder-class="placeholder">
				</view>
			</view>
			<view class="item flex mgb20">
				<view class="input">
					<input type="password" v-model="submitData.password" placeholder="密码" placeholder-class="placeholder">
				</view>
			</view>
			
			<view class="item submitbtn" style="padding: 0;border: 0;" @click="submit">
				<button class="Wbtn" type="submint">立即登录</button>
			</view>
		</view>
		
		
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				submitData:{
					login_name:'',
					password:''
				},
				showAll:false
			}
		},
		
		onLoad(options) {
			const self = this;
			if (uni.getStorageSync('staffToken')) {
				uni.redirectTo({
					url: '/pages/storeOrder/storeOrder'
				})
			}else{
				self.showAll = true
			}
		},
		
		methods: {
			
			submit() {
				const self = this;
			
				const postData = {
					login_name: self.submitData.login_name,
					password:self.submitData.password
				};
				if (self.$Utils.checkComplete(self.submitData)) {
					
					const callback = (res) => {
						if (res.solely_code == 100000) {
							console.log(res);
							uni.setStorageSync('staffToken', res.token);
							uni.setStorageSync('staffInfo', res.info);
							uni.redirectTo({
								url: '/pages/storeOrder/storeOrder'
							}) 
						} else {
							self.$Utils.showToast(res.msg,'none')
						}
					}
					self.$apis.shopLogin(postData, callback);
				} else {
					self.$Utils.showToast('请补全登录信息', 'none')
				};
			},
			
		},
	};
</script>

<style>
	
	page{padding-bottom: 60rpx;}	
	.loginBj{width: 440rpx;height: 440rpx;margin: 0 auto;}
	.loginBj image{width: 100%;height: 100%;}
	
	.loginCont{width:90%;margin: 0 auto;z-index: 2;background: #fff;padding: 0rpx 60rpx;box-sizing: border-box;}
	.loginCont .item{border-radius: 10rpx;height: 80rpx;box-sizing: border-box;padding: 0 30rpx;border: 1px solid #c3c3c3;}
	/* .loginCont .item .icon image{width: 36rpx;height: 36rpx;margin-right: 30rpx;} */
	.loginCont .item .input{width: 100%;}
	.loginCont .item .input input{font-size: 26rpx;height: 60rpx;}
</style>
