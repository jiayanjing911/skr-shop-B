<template>
	<view>
		<nologin-show></nologin-show>
	</view>
</template>

<script>
	import NologinShow from "@/pages/shopcar/noLoginShow.vue"
	export default {
		components: {
			NologinShow
		},
		data() {
			return {

			}
		},
		methods: {

		},
		// app h5支持
		// # ifndef MP-WEIXIN
		onNavigationBarButtonTap: function(e) {
			// console.log(e);
			if (e.onclick === 'goHome') {
				uni.switchTab({
					url: '/pages/home/home'
				})
			}
		},
		// # endif


	}
</script>

<style>

</style>
