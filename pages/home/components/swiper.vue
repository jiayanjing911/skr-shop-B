<template>
	<view>
		<view class="swiper">
			<view class="uni-margin-wrap">
				<!--  indicator-dots="false" -->
				<swiper class="swiper" indicator-dots="false" circular autoplay="true" interval="2000" duration="500">
					<block v-for="(item,index) in swiperImg" :key="index">
						<swiper-item>
							<view class="swiper-item">
								<image :src="item.swiperImg" mode="widthFix"></image>
							</view>
						</swiper-item>
					</block>
				</swiper>
			</view>
		</view>
	</view>
</template>

<script>
	import {
		getSwiper
	} from '@/api/home.js';
	export default {
		data() {
			return {
				swiperImg: []
			}
		},
		methods: {
			getSwiper() {
				getSwiper().then((data) => {
					// console.log(data.res);
					this.swiperImg = data.res;
				})
			},
		},
		created() {
			this.getSwiper()
		}
	}
</script>

<style scoped lang="scss">
	.swiper-item {
		display: block;
		text-align: center;
		
		image {
			width: 135%;
			height: 100%;
		}
	}
</style>
