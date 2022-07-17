<template>
	<view class="city">
		<view class="city-list" v-for="(item,index) in cityList" :key="index">
			<view class="letter">{{item.title}}</view>
			<view class="list">
				<view class="citys" v-for="v in item.city" :key="v.city">{{v}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				cityList:[],
			}
		},
		onLoad (){
			// 发送请求
			uni.request({
				url:'http://129.211.169.131:6368/citys',
				success:(res)=>{
					// 数据处理
					//获取到字母的数组。并进行去重[...new Set(数组)]，排序arr.sort()
					let letterArr=[...new Set(res.data.map(item=>item.city_pre.toUpperCase()))].sort();
					
					let newArr=letterArr.map(str=>{
						return {
							title:str,
							city:[]
						}
					});
					
					for (let item of res.data) {
						// 获取数据里的字母
						let letter=item.city_pre.toUpperCase()
						// 遍历数据修改的新数组
						for (let v of newArr) {
							// 如果新数组里面的字母和所有的字母进行对比，相同的就将对应的item对象添加进数组
							if(v.title===letter) v.city.push(item.city_name)
						}
					}
					this.cityList=newArr
					console.log(this.cityList);
					
					
					// console.log(letterArr);
					
					// console.log(res.data);
				}
			})
		},
		methods: {
			
		}
	}
</script>

<style lang="scss">
	.city{
		background-color: rgn(245, 245, 245);
		padding: 10px;
		.city-list{
			margin: 10px 0;
			.list{
				padding: 10px 0;
				display: flex;
				flex-wrap: wrap;
				background-color: #fff;
				.citys{
					width: 33%;
					display: flex;
					align-items: center;justify-content: center;
					padding: 4px 0;
				}
			}
		}
	}
</style>
