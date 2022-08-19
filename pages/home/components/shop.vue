<template>
	<view class="shop">
		<view>
			<scroll-view scroll-x="true" class="nav" :scroll-left="scrollX" @scroll="onscroll">
				<block v-for="(item,index) in navList" :key="index" :str="item">
					<text @click="chooseTab(index,item)" :class="{item:true,active:count===index}">{{item}}</text>
				</block>
			</scroll-view>
			<view class="content">
				<swiper :current="count" @change="swiperChenge" style="min-height: 100vh;">
					<block v-for="(item,index) in navList" :key="index">
						<swiper-item style="overflow: scroll;">
							<scroll-view :refresher-threshold="20" scroll-y="true" style="height: 100vh;"
								@scrolltolower="scrolly">
								<shop-view :shopDate="shopDate"></shop-view>
							</scroll-view>
						</swiper-item>
					</block>
				</swiper>
			</view>
		</view>
	</view>
</template>

<script>
	import {
		getParentName,
		getProduct
	} from '@/api/home.js'
	export default {
		name: "Shop",
		data() {
			return {
				navList: [],
				scrollX: 0, // 记录滚动条滚动的位置
				count: 0, // 记录当前点击点击的按钮
				top: 0,
				flag: true,
				str: '鞋类',
				num: 20,
				shopDate: [],
				numm: 0,
				flag: false,
				midDate: []
			}
		},
		methods: {
			onscroll(e) {
				this.scrollX = e.detail.scrollLeft
			},
			chooseTab(index, strs) {
				// console.log(strs);
				this.count = index;
				this.str = strs;
			},
			swiperChenge(e) {
				this.count = e.detail.current
			},
			// 分割res数组
			getNewArray(array, subGroupLength) {
				let index = 0;
				let newArray = [];
				while (index < array.length) {
					newArray.push(array.slice(index, (index += subGroupLength)));
				}
				return newArray;
			},
			// 获取一级分类
			getParentName() {
				getParentName().then((data) => {
					this.navList = data.data
				})
			},
			// 获取一级分类商品
			getProduct(obj) {
				getProduct(obj).then((data) => {
					this.midDate = this.getNewArray(data.res, this.num)
					this.shopDate = this.midDate[this.numm]
					console.log(this.shopDate);
				})
			},
			scrolly(e) {
				this.flag = true;
				this.numm++;
				// console.log(this.numm);
				setTimeout(() => {
					if (this.numm < this.midDate.length) {
						console.log(this.numm, this.midDate.length);
						this.shopDate = this.shopDate.concat(this.midDate[this.numm]);
						// console.log(this.shopDate);
					}
				}, 2000)
			}
		},
		created() {
			this.getParentName();
			this.getProduct({
				parent_name: this.str,
				start: 0,
				end: this.num
			})
		},
		watch: {
			str(news, old) {
				this.getProduct({
					parent_name: news,
					start: 0,
					end: this.num
				});
			},
			count(val) {
				if (val < 3) {
					this.scrollX = 0
				} else {
					if (!this.flag) return
					this.flag = false
					// 获取手机宽度 找到响应式和固定的单位
					uni.getSystemInfo({
						success: (res) => {
							let scrollx = this.scrollX;
							// res 可以获取系统 屏幕 设备id 品牌...
							// 获取视图窗口或者屏幕的宽度
							let screewidth = parseInt(res.screenWidth);
							let move = (val - 3) * 93 + 138;
							let b = move / 375 * screewidth;
							// this.scrollX = b;
							// 动画过渡实现	 (将js阶段的动画插件,改成uniapp插件/uniapp工具函数,然后使用)
							let timer = null;
							if (b - scrollx > 0) {
								// 向右走
								timer = setInterval(() => {
									this.scrollX += 5;
									if (this.scrollX >= b) {
										this.scrollX = b;
										clearInterval(timer);
										this.flag = true
									}
								}, 16.7)
							} else {
								// 向左走
								timer = setInterval(() => {
									this.scrollX -= 5;
									if (this.scrollX <= b) {
										this.scrollX = b;
										clearInterval(timer)
										this.flag = true
									}
								}, 16.7)
							}
						}
					})


				}
			}
		}
	}
</script>

<style lang='scss' scoped>
	.shop {
		background-color: #f5f4f9;

		.nav {
			z-index: 100;
			display: inline-block;
			white-space: nowrap;
			width: 100%;
			height: 38px;
			line-height: 35px;
			background-color: white;
			position: sticky;
			top: 0px;

			text {
				color: #646566;
				font-size: 20rpx;
				width: 14%;
				margin: 0 5%;
			}
		}

		.content {
			padding-top: 15px;
		}
	}

	.item {
		display: inline-block;
		width: 187rpx;
		font-size: 16px;
		text-align: center;
	}

	.active {
		font-size: 22rpx;
		font-weight: 600;
		color: purple;
		border-bottom: 3px solid rgb(204, 204, 204);
		transition-duration: 0.3s;
	}
</style>
