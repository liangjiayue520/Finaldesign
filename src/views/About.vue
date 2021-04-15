<style scoped>
	#chart_example {
		width: 100%;
		height: 400px;
		margin: 0 auto;
	}

	#thebody {
		width: 100%;
		height: 100%;
		top: 0px;
		/* background-color: #151515; */
	}
	.feeling{
		margin-bottom:-4px;
	}
</style>
<template>
	<div id="thebody">
		<el-row>
			<el-col :span="12">
				<iframe frameborder="no" src="static/2020-2月份.html" style=" width: 100%;height:800px"></iframe>
			</el-col>
			<el-col :span="12">
				<iframe frameborder="no" src="static/wordcloud.html" style=" width: 100%;height:800px"></iframe>
			</el-col>
		</el-row>
		<el-row>
			<el-col :span="24">
				<div class="feeling"></div>
				<iframe frameborder="no" src="static/情感指数.html" style=" width: 100%;height:800px"></iframe>
			</el-col>
		</el-row>
	</div>
</template>

<script>
	export default {
		data() {
			return {}
		},
		mounted() {
			//申明echarts实例 获取div中的id	
			let echarts = require('echarts');
			let myChart = echarts.init(document.getElementById('chart_example'), "dark");
			let option = {
				color: ['#00aa00'],
				tooltip: {
					trigger: 'axis',
					axisPointer: {
						type: 'shadow'
					}
				},
				xAxis: [{
					type: 'category',
					data: ['1月', '2月', '3月', '4月', '5月', '6月', '7月', '8月', '9月', '10月', '11月', '12月', ],
					axisTick: {
						alignWithLabel: true
					}
				}],
				yAxis: [{
					type: 'value'
				}],
				series: [{
					name: '每月花费',
					type: 'bar',
					barWidth: '60%',
					data: [995, 666, 444, 858, 654, 236, 645, 546, 846, 225, 547, 356]
				}]
			};
			myChart.setOption(option);

			// 建议加上以下这一行代码， 不加的效果图如下（ 当浏览器窗口缩小的时候）。 超过了div的界限（ 红色边框）
			window.addEventListener('resize', function() {
				myChart.resize()
			});
		},
	}
</script>
