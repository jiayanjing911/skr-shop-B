<template>
	<div class="top">
		<div class="topImg" ref="topImg">
			<img src="@/static/img/homeTop.png">
			<!-- <span>搜索</span> -->
		</div>
		<div class="btn" @click="goSearch">
			<img src="@/static/icons/search1.png">
		</div>
	</div>
</template>

<script>
	// import a from '@/static/img/homeTop.png'
	export default {
		name: 'homeTop',
		mounted() {
			let Edg = 0
			setInterval(() => {
				Edg += 180
				this.$refs.topImg.style.transform = `rotateX(${Edg}deg)`;
			}, 2000)
		},
		methods: {
			// 点击搜索跳转搜索页面
			goSearch(){
				uni.navigateTo({
					url: '/pages/search/search'
				})
			}
		}
	}
</script>

<style scoped lang="scss">
	.top {
		background-color: rgb(50, 50, 50);
		color: rgb(255, 255, 255);
		text-align: center;
		padding:8px 0;
		position: relative;

		.topImg {
			transition: all 2s;
			// transform-style: preserve-3d;
			// transform: rotateX(0deg);
			// padding-bottom: 5px;
			vertical-align: top;
			line-height: 15px;

			img {
				width: 25%;
			}
		}
		.btn{
			position: absolute;
			top: 11px;
			right: -80px;
			img{
				width: 10%;
			}
		}
	}
</style>
