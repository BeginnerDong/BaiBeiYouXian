<template>
	<view>

		<view class="banner-box">
			<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000"
			 indicator-active-color="#757575">
				<block v-for="(item,index) in mainData.bannerImg" :key="index">
					<swiper-item class="swiper-item">
						<image :src="item.url" class="slide-image" />
					</swiper-item>
				</block>
			</swiper>
		</view>
		<view class="mglr4 pdtb15">
			<view class="fs16 ftw">{{mainData.title}}</view>
			<view class="color6 fs12 pdt5">提货时间：{{Utils.timeto(today,'md')}}</view>
			<view class="fs16 ftw red mgt10 ftw">￥{{mainData.price?mainData.price:''}}</view>
		</view>

		<view class="f5H10"></view>


		<view class="pdt15">
			<view class="mglr4 xqInfor">
				<view class="fs15 ftw pdb20">商品规格描述</view>
				<view class="cont fs14 center">
					<view class="content ql-editor" style="padding:0;" v-html="mainData.content">
					</view>
				</view>
			</view>
		</view>

		<!-- 预算中 -->
		<view class="xqbotomBar center" style="height: 100rpx;" v-if="mainData.is_notBuying&&!mainData.is_noStock">
			<view class=" flex fs12">
				<view class="ite flexColumn" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
					<image src="../../static/images/detsilsl-icon1.png" mode=""></image>
					<view class="mgt5">首页</view>
				</view>
				<button class="ite flexColumn" open-type="share">
					<image src="../../static/images/detsilsl-icon3.png" mode=""></image>
					<view class="mgt5">分享</view>
				</button>
			</view>
			<view class="payBtn" style="width: 360rpx;">预售中</view>
		</view>

		<!-- 已售罄 -->
		<view class="xqbotomBar center" style="height: 100rpx;" v-if="mainData.is_noStock">
			<view class=" flex fs12">
				<view class="ite flexColumn" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
					<image src="../../static/images/detsilsl-icon1.png" mode=""></image>
					<view class="mgt5">首页</view>
				</view>
				<button class="ite flexColumn" open-type="share">
					<image src="../../static/images/detsilsl-icon3.png" mode=""></image>
					<view class="mgt5">分享</view>
				</button>
			</view>
			<view class="payBtn" style="width: 360rpx;background: #bdbcbc;">已售罄</view>
		</view>

		<!-- 抢购中 -->
		<view class="xqbotomBar center" style="height: 100rpx;" v-if="mainData.is_Buying&&!mainData.is_noStock">
			<view class=" flex fs12">
				<view class="ite flexColumn" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
					<image src="../../static/images/detsilsl-icon1.png" mode=""></image>
					<view class="mgt5">首页</view>
				</view>
				<view class="ite flexColumn" @click="addCar">
					<image src="../../static/images/detsilsl-icon2.png" mode=""></image>
					<view class="mgt5">加入购物车</view>
				</view>
				<button class="ite flexColumn" @tap="shareEvn">
					<image src="../../static/images/detsilsl-icon3.png" mode=""></image>
					<view class="mgt5">分享</view>
				</button>
			</view>
			<view class="payBtn" @click="goBuy()">立即购买</view>
		</view>
		<hchPoster ref="hchPoster" :canvasFlag.sync="canvasFlag" @cancel="canvasCancel" :posterObj.sync="posterData" />
		<view :hidden="canvasFlag">
			<!-- 海报 要放外面放组件里面 会找不到 canvas-->
			<canvas class="canvas" canvas-id="myCanvas"></canvas><!-- 海报 -->
		</view>

		<view class="share-pro"  v-if="deliveryFlag">
			<view class="share-pro-mask"></view>
			<view class="share-pro-dialog" :class="deliveryFlag?'open':'close'">
				<view class="close-btn" @tap="closeShareEvn">
					<span class="font_family">&#xe6e6;</span>
				</view>
				<view class="share-pro-title">分享</view>
				<view class="share-pro-body">
					<view class="share-item">
						<button open-type="share">
							<view class="font_family share-icon">&#xe6fa;</view>
							<view>分享给好友</view>
						</button>
					</view>
					<view class="share-item" @tap="createCanvasImageEvn">
						<view class="font_family share-icon">&#xe6f9;</view>
						<view>生成分享图片</view>
					</view>
				</view>

			</view>
		</view>
	</view>
</template>

<script>
	import hchPoster from '../../wxcomponents/hch-poster/hch-poster.vue'
	export default {
		components: {
			hchPoster,
		},
		data() {
			return {
				Router: this.$Router,
				Utils: this.$Utils,
				curr: 1,
				messageData: [{}, {}, {}, {}],
				mainData: {},
				today: 0,
				deliveryFlag: false,
				canvasFlag: true,
				posterData: {}
			}
		},

		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.today = self.today = new Date().getTime() + uni.getStorageSync('user_info').thirdApp.pickup_time * 86400000;
			self.$Utils.loadAll(['getMainData','getQrData'], self);
		},

		onShow() {
			const self = this;
			self.orderList = [];
			uni.removeStorageSync('payPro');
		},

		onShareAppMessage(ops) {
			console.log(ops)
			const self = this;
			if (ops.from === 'button') {
				return {
					title: self.mainData.title+' '+self.mainData.price+'元',
					path: '/pages/productDetail/productDetail?id=' + self.mainData.id, //点击分享的图片进到哪一个页面
					imageUrl: self.mainData.mainImg[0] ? self.mainData.mainImg[0].url : '',
					success: function(res) {
						// 转发成功
						console.log("转发成功:" + JSON.stringify(res));
					},
					fail: function(res) {
						// 转发失败
						console.log("转发失败:" + JSON.stringify(res));
					}
				}
				console.log(ops.target)
			} else {
				return {
					title: self.mainData.title+' '+self.mainData.price+'元'	,
					path: '/pages/productDetail/productDetail?id=' + self.mainData.id, //点击分享的图片进到哪一个页面
					imageUrl: self.mainData.mainImg[0] ? self.mainData.mainImg[0].url : '',
					success: function(res) {
						// 转发成功
						console.log("转发成功:" + JSON.stringify(res));
					},
					fail: function(res) {
						// 转发失败
						console.log("转发失败:" + JSON.stringify(res));
					}
				}
				console.log(ops.target)
			}

		},

		methods: {
			
			
			getQrData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken'
				postData.qrInfo = {
					scene: self.mainData.id,
					path: 'pages/productDetail/productDetail',
				};
				postData.output = 'url';
				postData.ext = 'png';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.QrData = res;
					} else {
						self.$Utils.showToast(res.msg, 'none')
					}
					self.$Utils.finishFunc('getQrData');
				};
				self.$apis.getQrCode(postData, callback);
			},

			createCanvasImageEvn() {
				const self = this;
				console.log(self.QrData.info.url)
				Object.assign(this.posterData, {
					url: self.mainData.mainImg[0].url, //商品主图
					icon: 'https://img0.zuipin.cn/mp_zuipin/poster/hch-hyj.png', //醉品价图标
					title: self.mainData.title, //标题
					discountPrice: self.mainData.price, //折后价格
					//orignPrice: "300.00", //原价
					code: self.QrData.info.url, //小程序码
				})
				this.$forceUpdate(); //强制渲染数据
				setTimeout(() => {
					this.canvasFlag = false; //显示canvas海报
					this.deliveryFlag = false; //关闭分享弹窗
					this.$refs.hchPoster.createCanvasImage(); //调用子组件的方法
				}, 1000)
			},

			// 分享弹窗
			shareEvn() {
				this.deliveryFlag = true;
			},
			// 关闭分享弹窗
			closeShareEvn() {
				this.deliveryFlag = false;
			},
			// 取消海报
			canvasCancel(val) {
				this.canvasFlag = val;
			},


			addCar() {
				const self = this;
				var array = self.$Utils.getStorageArray('cartData');
				for (var i = 0; i < array.length; i++) {
					if (array[i].id == self.id) {
						var target = array[i]
					}
				}
				if (target) {
					target.count = target.count + 1
				} else {
					var target = self.mainData;
					target.count = 1;
					target.isSelect = true;
				}
				self.$Utils.showToast('加入成功', 'none');
				self.$Utils.setStorageArray('cartData', target, 'id', 999);
			},

			goBuy() {
				const self = this;
				uni.setStorageSync('canClick', false);
				self.orderList.push({
					product_id: self.mainData.id,
					count: 1,
					type: self.mainData.type,
					product: self.mainData
				}, );
				uni.setStorageSync('payPro', self.orderList);
				self.Router.navigateTo({
					route: {
						path: '/pages/orderConfim/orderConfim'
					}
				})
				uni.setStorageSync('canClick', true);
			},

			getMainData() {
				const self = this;
				var now = new Date().getTime();
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					start_time: ['<', now],
					id: self.id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
						if (self.mainData.stock == 0 || self.mainData.sellout == 1) {
							self.mainData.is_noStock = true
						};
						if (parseInt(self.mainData.end_time) > now) {
							self.mainData.is_notBuying = true
						} else {
							self.mainData.is_Buying = true
						}
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},

		}
	};
</script>

<style lang="scss">
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/detail.css";

	page {
		padding-bottom: 140rpx;
	}

	.swiper-box {
		height: 400rpx;
		box-sizing: border-box;
		overflow: hidden;
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

	.orderNav .tt.on {
		font-weight: bold;
	}

	.orderNav .tt.on::after {
		bottom: 6rpx;
	}

	.shareBtn {
		background: #F5F5F5;
		width: 120rpx;
		height: 44rpx;
		border-radius: 25rpx 0 0 25rpx;
	}

	.xqbotomBar {
		z-index: 1;
	}

	button {
		background: none;
		line-height: 1.5;
		margin-left: 0;
		margin-right: 0;
		font-size: 15px;
		border-radius: 0;
	}

	button::after {
		border: none;
	}

	.button-hover {
		color: #000000;
		background: none;
	}

	@font-face {
		font-family: 'font_family';
		/* project id 1065286 */
		src: url('//at.alicdn.com/t/font_1065286_3bsye5aijur.eot');
		src: url('//at.alicdn.com/t/font_1065286_3bsye5aijur.eot?#iefix') format('embedded-opentype'),
			url('//at.alicdn.com/t/font_1065286_3bsye5aijur.woff2') format('woff2'),
			url('//at.alicdn.com/t/font_1065286_3bsye5aijur.woff') format('woff'),
			url('//at.alicdn.com/t/font_1065286_3bsye5aijur.ttf') format('truetype'),
			url('//at.alicdn.com/t/font_1065286_3bsye5aijur.svg#font_family') format('svg');
	}

	.font_family {
		font-family: "font_family" !important;
		font-size: 16px;
		font-style: normal;
		-webkit-font-smoothing: antialiased;
		-webkit-text-stroke-width: 0.2px;
		-moz-osx-font-smoothing: grayscale;
	}

	/* 按钮去掉边框 */
	button::after {
		border: none;
	}

	button {
		margin-left: 0;
		margin-right: 0;
		padding-left: 0;
		padding-right: 0;
		line-height: 1;
		color: #1c1c1c;
		font-size: 28rpx;
		background: none;
	}

	.button-hover {
		color: #1c1c1c;
		background: none;
	}

	/*每个页面公共css */
	.content {
		text-align: center;
		height: 100%;
	}
	.share-btn{
		padding: 30upx 60upx;background-color:$uni-btn-color;color: $uni-text-color-inverse;
	}
	.share-pro{
	    display: flex;
	    align-items: center;
	    justify-content: flex-end;
	    flex-direction: column;
	    z-index: 5;
	    line-height: 1;
	    box-sizing: border-box;
	    .share-pro-mask{
	        width: 100%;
	        height: 100%;
	        position: fixed;
	        top: 0;
	        right: 0;
	        bottom: 0;
	        left: 0;
	        background: rgba(0, 0, 0, 0.5);
	    }
	    .share-pro-dialog {
	        width: 750rpx;
	        height: 310rpx;
	        overflow: hidden;
	        background-color: #fff;
	        border-radius: 24rpx 24rpx 0px 0px;
	        position: relative;
	        box-sizing: border-box;
	        position: fixed;
	        bottom: 100rpx;
	        .close-btn {
	            padding: 20rpx 15rpx;
	            position: absolute;
	            top: 0rpx;
	            right: 29rpx;
	        }
	        .share-pro-title {
	            font-size: 28rpx;
	            color: #1c1c1c;
	            padding: 28rpx 41rpx;
	            background-color: #f7f7f7;
	        }
	
	        .share-pro-body{
	            display: flex;
	            flex-direction: row;
	            justify-content:space-around;
	            font-size: 28rpx;
	            color: #1c1c1c;
	            .share-item{
	                display: flex;
	                flex-direction:column;
	                justify-content: center;
	                justify-content:space-around;
	                .share-icon{
	                    text-align:center;
	                    font-size: 70rpx;
	                    margin-top: 39rpx;
	                    margin-bottom: 16rpx;
	                    color: #42ae3c;
	                }
	                &:nth-child(2){
	                    .share-icon{
	                        color: #ff5f33;
	                    }
	                }
	            }
	        }
	    }
	
	    /* 显示或关闭内容时动画 */
	
	    .open {
	        transition: all 0.3s ease-out;
			transform: translateY(0);
	    }
	
	    .close {
	        transition: all 0.3s ease-out;
			transform: translateY(310rpx);
	    }
	
	}
	 .canvas{
	    position: fixed !important;
	    top: 0 !important;
	    left: 0 !important;
	    display: block !important;
	    width: 100% !important;
	    height: 100% !important;
	    z-index: 10;
	}
</style>
