<template>
	<view class="content">
		<!-- 头部 -->
		<Header></Header>
		<!-- 轮播图 -->
		<swiper indicator-dots 	indicator-color='#fff' 	indicator-active-color='#04a75b' autoplay circular>
			<swiper-item>
				<image src="../../static/img/2.webp" mode=""></image>
			</swiper-item>
			<swiper-item>
				<image src="../../static/img/3.webp" mode=""></image>
			</swiper-item>
			<swiper-item>
				<image src="../../static/img/4.webp" mode=""></image>
			</swiper-item>
			<swiper-item>
				<image src="../../static/img/5.webp" mode=""></image>
			</swiper-item>
			<swiper-item>
				<image src="../../static/img/6.webp" mode=""></image>
			</swiper-item>
		</swiper>
		<!-- 功能导航 -->
		
		<!-- 房源列表 -->
		<view class="houese-main">
			<view @click="clickHouese(index)" class="houese-list" v-for="(item,index) in houeseList" :key="item.id">
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
		<view v-show="isShow">我也是有底线的！！</view>
		
		
	</view>
</template>
 
<script>
	export default {
		data() {
			return {
				page:1,
				pagesize:10,
				houeseList:[],
				isShow:false,
			}
		},
		onLoad() {
			this.render(this.page)	
		},
		onReachBottom(){
			if(this.isShow===false){
				this.render(++this.page)
			}
			
		},
		
		methods: {
			render(n){
				// 发送请求
				uni.request({
					url:'http://129.211.169.131:6368/gethouse',
					// 传送参数
					data:{
						page:n,
						pagesize:this.pagesize,
					},
					// 请求成功得到数据
					success:(res)=>{
						// 如果得到的是一个空数组，将显示按钮打开
						if(res.data.data.length==0) this.isShow=true
						// console.log(111);
						// console.log(res.data.data);
						// 将得到的数据赋值给data数据，并且解构，因为res.data.data是一个数组，
						// 放进数组中就会变成数组套数组，所以需要解构
						this.houeseList.push(...res.data.data)
					}
				})
			},
			clickHouese(index){
				// 本地存储uni.setStorageSync(KEY,DATA)
				// 获取本地存储的数据，有数据就赋值给数组，没有数据就等于空
				let arr = uni.getStorageSync('houeseList') || [];
				//数组过滤,过滤有相同的数据
				let filterArr=this.houeseList[index];
				for(let i=0;i<arr.length;i++){
					if(arr[i].name===filterArr.name){
						arr.splice(i,1)
					}
				}
				// 数据解构赋值，并存储
				uni.setStorageSync('houeseList',[this.houeseList[index],...arr])
				
			}
		}
	}
</script>

<style lang="scss">
	swiper{
		width: 100%;
		height: 200px;
		image{
			height: 200px;
			width: 100%;
		}
	}
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
