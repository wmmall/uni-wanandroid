<template>
	<view>
		<web-view v-bind:src="url"></web-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				url: ''
			};
		},
		onLoad(e) {
			this.url = e.url;
			uni.setNavigationBarTitle({
				title: e.title? e.title : '玩android'
			})
		}
	}
</script>
	
<style>

</style>
