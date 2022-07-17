<template>
	<view>
		<view class="houese-main">
			<view class="houese-list" v-for="(item,index) in historyList" :key="item.id">
				<image :src="item.imgs" mode=""></image>
				<view class="houese-text">
					<view class="title">{{item.name}}</view>
					<view class="address">{{item.area}}  {{item.range}}</view>
					<view class="area">{{item.type}},{{item.point}}平</view>
				</view>
				<view class="price">
					￥{{item.price}}元/平
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				historyList:[],
			}
		},
		onShow() {
			let data=uni.getStorageSync('houeseList');
			this.historyList=data
		},
		methods: {
			
		}
	}
</script>

<style lang="scss">
.houese-main{
		padding: 10px;
		.houese-list{
			display: flex;
			margin-bottom: 10px;
			image{
				width: 100px;
				height: 100px;
			}
			.houese-text{
				flex: 1;
				padding-left: 10px;
				color: #999;
				font-size: 14px;
				.title{
					font-weight: bold;
					color: #222;	
					font-size: 16px;
				}
				.address{
					padding: 10px 0;
				}
			}
			.price{
				display: flex;
				justify-content: center;
				align-items: center;
				color: red;
				padding-right: 10px;
			}
		}
		
	}
</style>
