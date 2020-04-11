<template>
	<view>
		<view class="mglr4 mgt15">
			
			<view class="whiteBj radius10 boxShaow oh mgb15 pdb15">
				<view class="pdlr4pdb15">
					<view class="flex seltTake center fs15 color6">
						<view class="btn" :class="curr==1?'on':''" @click="currChange('1')">到店自提</view>
						<view class="btn" :class="curr==2?'on':''" @click="currChange('2')">送货上门</view>
					</view>
				
					<view class="pdlr4 radius10" v-show="curr==1">
						<view class="flexRowBetween pdtb15 borderB1" @click="Router.navigateTo({route:{path:'/pages/zitiAddress/zitiAddress'}})">
							<view class="fs14 color6">
								<view class="flex mgt5">
									<view class="mgr15 color2">门店名称门店名称</view>
									<view>15689562352</view>
								</view>
								<view class="mgt5">陕西省西安市雁塔区长安区韦曲街道</view>
							</view>
							<view class="flexEnd" style="width: 20%;">
								<image class="arrowR" src="../../static/images/addressl-icon2.png" mode=""></image>
							</view>
						</view>
						<view class="flexRowBetween pdt15">
							<view class="editMsg"><input type="text" value="" placeholder="请输入姓名" placeholder-class="placeholder" /></view>
							<view class="editMsg"><input type="text" value="" placeholder="请输入姓名" placeholder-class="placeholder" /></view>
						</view>
					</view>
					<view class="pdlr4 radius10" v-show="curr==2">
						<view class="flexRowBetween  pdtb15 borderB1" @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
							<view class="fs14 color6 flex">
								<view style="width: 36rpx;height: 36rpx;margin-right: 10rpx;"><image style="" src="../../static/images/shoppimg-icon2.png" mode=""></image></view>
								<view class="red">选择收货地址</view>
							</view>
							<view class="flexEnd" style="width: 20%;">
								<image class="arrowR" src="../../static/images/addressl-icon2.png" mode=""></image>
							</view>
						</view>
						<view class="flexRowBetween  pdtb15 borderB1"  @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
							<view class="fs14 color6">
								<view class="flex mgt5">
									<view class="mgr15 color2">张思德</view>
									<view>15689562352</view>
								</view>
								<view class="mgt5">陕西省西安市雁塔区高新大都荟</view>
							</view>
							<view class="flexEnd" style="width: 20%;">
								<image class="arrowR" src="../../static/images/addressl-icon2.png" mode=""></image>
							</view>
						</view>
						<view class="pdt15 red">5公里范围内均可配送</view>
					</view>
				</view>
			</view>
			<view class="proRow mgt15">
				<view class="item flexRowBetween borderB1" >
					<view class="pic"><image src="../../static/images/shopping-img.png" mode=""></image></view>
					<view class="infor">
						<view class="tit">墨西哥牛油果8枚单果200g左右</view>
						<view class="flexRowBetween B-price">
							<view class="price ftw">56</view>
							<view class="flexEnd">
								<view class="numBox flex">
									<view class="btn" @click="counter('-')">-</view>
									<view class="num">{{count}}</view>
									<view class="btn pubBj white add" @click="counter('+')">+</view>
								</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		
		<view class="xqbotomBar">
			<view class="flex mgl15 fs13">总计：<view class="price fs14">126</view></view>
			<view class="payBtn" v-show="curr==1" @click="payShowShow">立即支付</view>
			<view class="payBtn" v-show="curr==2">立即支付</view>
		</view>
		
		<!-- 弹框 -->
		<view class="black-bj" v-show="is_show"></view>
		
		<view class="xieyiShow payShow whiteBj radius10" v-show="is_payShow">
			<view class="center pdtb15 color6">订单确认</view>
			<view class="">
				<view class="borderB1"></view>
				<view class="borderB1 pdlr4 pdt15 pdb15 fs13">
					<view class="mgb10">门店名称：商务二上档次尅是积极</view>
					<view class="mgb10">门店电话：15623562356</view>
					<view class="">门店地址：陕西省西安市雁塔区高新大都荟</view>
				</view>
				<view class="center flexRowBetween">
					<view style="width: 50%; height: 90rpx;line-height: 90rpx;color:#58a3ff ;box-sizing: border-box;border-right: 1px solid #eee;" @click="payShowShow">重新输入</view>
					<view style="width: 50%;height: 90rpx;line-height: 90rpx;" class="pubColor" @click="Router.navigateTo({route:{path:'/pages/orderPayOk/orderPayOk'}})">确认</view>
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
				showView: false,
				score:'',
				wx_info:{},
				is_show:false,
				curr:1,
				count:1,
				is_show:false,
				is_payShow:false
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			currChange(curr){
				const self = this;	
				if(curr!=self.curr){
					self.curr = curr
				}
			},
			counter(type) {
				const self = this;			
				if (type == '+') {
					self.count++;
				} else {
					if (self.count > 1) {
						self.count--;
					}
				};			
				self.countTotalPrice();
			},
			xieyiShow(){
				const self = this;
				self.is_show = !self.is_show
				self.is_xieyiShow = !self.is_xieyiShow
			},
			payShowShow(){
				const self = this;
				self.is_show = !self.is_show
				self.is_payShow = !self.is_payShow
			}
		}
	};
</script>

<style>
	@import "../../assets/style/detail.css";
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;}
	
	.editMsg{width: 45%;height: 60rpx;padding: 0 10rpx;box-sizing: border-box;border: 1px solid #eee;box-sizing: border-box;text-align: center;}
	.editMsg input{width: 100%;height: 60rpx;line-height: 60rpx;box-sizing: border-box;text-align: center;font-size: 26rpx;}
	
	.seltTake .btn{width: 50%;height: 80rpx;line-height: 80rpx;background: #f8f8f8;}
	.seltTake .btn.on{background: #fff;}
	.seltTake .btn:last-child.on{border-radius: 40rpx 0 0 0;}
	
	.proRow .item .pic{width: 140rpx; height: 140rpx;}
	.proRow .item .infor{width: 75%; height: 140rpx;}
	
	
	.xieyiShow{width: 90%;min-height: 300rpx;position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);z-index: 50;box-sizing: border-box;}
	.xieyiShow .xqInfor .cont{height: 400rpx;padding-bottom: 80rpx;}
	
	
</style>
