<template>
	<view>
		<view class="header-top">
			<view class="header" :style="{paddingTop:top+'px',paddingRight:right-left+'px'}">
				<navigator url="/pages/city/city" class="city">{{city}}</navigator url="">
				<view class="serach" :style="{height:height+'px'}">你想住在哪里？</view>
				<navigator url="/pages/map/map">
					<image src="../static/img/map.png" mode=""></image>
				</navigator>
			</view>
		</view>
		<view :style="{height:top+height+10+'px'}"></view>
	</view>
</template>

<script>
	export default {
		// name:"header",
		data() {
			return {
				top:'',
				left:'',
				height:'',
				right:'',
				city:'获取定位...',
			};
		},
		created(){
			//获取胶囊按钮信息
			let menuinfo=uni.getMenuButtonBoundingClientRect()
			this.top=menuinfo.top;
			this.left=menuinfo.left;
			this.height=menuinfo.height-1;
			this.right=menuinfo.right;
			//获取经度纬度信息
			uni.getLocation({
				type: 'wgs84',
				success: (res) =>{
					uni.request({
						url:'https://restapi.amap.com/v3/geocode/regeo?parameters',
						data:{
							key:'283028fae3f15ad84e08f7bf259e59a7',
							location: res.longitude + ',' + res.latitude
						},
						success: (res) => {
							console.log(res.data);
						       this.city = res.data.regeocode.addressComponent.district
						
						      }
					})
				},
			});
			
			
		},
		
	}
</script>

<style lang="scss">
	.header-top{
		background-color: #00a261;
		padding-bottom: 10px;
		font-size: 14px;
		position: fixed;
		left: 0;
		top: 0;
		right: 0;
		z-index: 999;
	}
	.header{
		
		display: flex;
		align-items: center;
		justify-content: space-between;
		.city{
			text-align: center;
			color: #fff;
			padding: 0 7px;
		}
		.serach{
			flex: 1;
			text-align: center;
			background-color: #fff;
			border-radius: 20px;
			line-height: 30px;
			color: #999;
		}
		image{
			height: 25px;
			width: 25px;
			padding: 0 16px 0 10px;
		}
	}
</style>