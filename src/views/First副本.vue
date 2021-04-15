<style scoped>
	#mapBox {
		width: 100%;
		height: 400px;
	}
	#block{
		text-align: left;
		height: 10px;
		background-color: #000000;
	}
	#homebox{
				position: relative;
				width: 100%;	
				height: 87.5rem;
				top: 0px;
				background-color: #323232;
	}
	.input-try{
		background-color: #323232;
		margin-bottom:-4px;
	}
/* 	#barline{
		width: 300px;
		height: 200px;
	} */
</style>
<template>
	<div id="homebox">
		<div id="block">
			<el-date-picker v-model="value1" color="#323232" type="date" placeholder="时间选择" value-format="yyyyMMdd" @change="geteverydayjson()">
			</el-date-picker>
		</div>
		<el-row justify="space-between">
			<el-col :span="12">
				<div id="mapBox"></div>
			</el-col>
			<el-col :span="12">
				<el-row>
					<div class="springmove"></div>
					<iframe frameborder="0"    src="static/春运迁徙.html" style=" width: 100%;height:200px"></iframe>
				</el-row>
				<el-row >
					<div class="input-try"></div>
					<iframe frameborder="0"   src="static/input-try.html" style=" width: 100%;height:200px"></iframe>
				</el-row>
			</el-col>
		</el-row>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				pickerOptions: {
					disabledDate(time) {
						return time.getTime() > Date.now();
					}
				},
				value1: '', //注意此处的value1的属性是string 
				datejson: [],
				everynum: [], //用来存放每天的数据从14号到28 共15组 二维数组
				//HFdata: [], //用来存放map特需的数据格式 二维数组
				HFdata: [ //数据中name的城市名称必须与geoCoordMap中城市名称一致, 不然关联不上，合肥到各地区的线路
					[{
						name: '湖北'
					}, {
						name: '广东',
						value: 9.54
					}],
					[{
						name: '湖北'
					}, {
						name: '浙江',
						value: 10.83
					}],
					[{
						name: '湖北'
					}, {
						name: '江苏',
						value: 7.37
					}],
					[{
						name: '湖北'
					}, {
						name: '湖南',
						value: 6.88
					}],
					[{
						name: '湖北'
					}, {
						name: '福建',
						value: 5.02
					}],
					[{
						name: '湖北'
					}, {
						name: '上海',
						value: 4.96
					}],
					[{
						name: '湖北'
					}, {
						name: '河南',
						value: 4.72
					}],
					[{
						name: '湖北'
					}, {
						name: '江西',
						value: 3.71
					}],
					[{
						name: '湖北'
					}, {
						name: '北京',
						value: 3.28
					}],
					[{
						name: '湖北'
					}, {
						name: '重庆',
						value: 3.16
					}],
					[{
						name: '湖北'
					}, {
						name: '安徽',
						value: 2.28
					}],
					[{
						name: '湖北'
					}, {
						name: '四川',
						value: 2.13
					}],
					[{
						name: '湖北'
					}, {
						name: '陕西',
						value: 2.00
					}],
					[{
						name: '湖北'
					}, {
						name: '山东',
						value: 1.59
					}],
					[{
						name: '湖北'
					}, {
						name: '广西',
						value: 1.34
					}],
					[{
						name: '湖北'
					}, {
						name: '河北',
						value: 1.29
					}],
					[{
						name: '湖北'
					}, {
						name: '贵州',
						value: 1.14
					}],
					[{
						name: '湖北'
					}, {
						name: '云南',
						value: 1.13
					}],
					[{
						name: '湖北'
					}, {
						name: '天津',
						value: 0.59
					}],
					[{
						name: '湖北'
					}, {
						name: '海南',
						value: 0.58
					}],
					[{
						name: '湖北'
					}, {
						name: '山西',
						value: 0.57
					}],
					[{
						name: '湖北'
					}, {
						name: '辽宁',
						value: 0.37
					}],
					[{
						name: '湖北'
					}, {
						name: '甘肃',
						value: 0.33
					}],
					[{
						name: '湖北'
					}, {
						name: '新疆',
						value: 0.24
					}],
					[{
						name: '湖北'
					}, {
						name: '内蒙古',
						value: 0.18
					}],
					[{
						name: '湖北'
					}, {
						name: '黑龙江',
						value: 0.16
					}],
					[{
						name: '湖北'
					}, {
						name: '吉林',
						value: 0.15
					}],
					[{
						name: '湖北'
					}, {
						name: '宁夏',
						value: 0.12
					}],
					[{
						name: '湖北'
					}, {
						name: '青海',
						value: 0.10
					}],
					[{
						name: '湖北'
					}, {
						name: '西藏',
						value: 0.03
					}],
				]
			}
		},

		created() {
			this.datejson = require('/public/static/武汉迁出14-28号.json') //获取json数据成功
			this.seteverynum();
		},

		methods: {
			//验证打印数据
			dataprint() {
				console.log(this.everynum);
			},

			//设定everynum,HFdata为二维数组 并且数据在元素渲染之前处理完毕
			seteverynum() {
				for (let i = 0; i < 15; i++)
					this.everynum[i] = new Array();
				// for (let i = 0; i < 30; i++)
				// 	this.HFdata[i] = new Array();
			},
			// 获取datepicker给出的当天的数据 date ratio  province的类型都是string
			geteverydayjson() {
				let flag = this.datejson[0]['date'];
				let kar = 0;
				for (let i = 0; i < 447; i++) {
					// console.log(typeof(this.datejson[i]['province'])); 
					if (this.datejson[i]['date'] == flag) {
						this.everynum[kar].push(this.datejson[i]);
					} else {
						flag = this.datejson[i]['date'];
						kar++;
						this.everynum[kar].push(this.datejson[i]);
					}
				}
				var newdate = this.value1
				let midday, len;
				for (let i = 0; i < 15; i++) {
					if (this.everynum[i][1].date == newdate) //成功获取到当天数据
					{
						midday = i; // 11 12 13
						if (i == 11 || i == 12 || i == 13)
							len = 29;
						else
							len = 30;
					}
				}
				// if (this.HFdata[0][1].value != 29.54 && this.HFdata[0][1].name != "广东") {
				// 	for (let i = 0; i < 30; i++) {
				// 		this.HFdata[i].length = 0;
				// 	}
				// }
				for (let i = 0; i < len; i++) {
					this.HFdata[i] = [{
						name: '湖北'
					}, {
						name: this.everynum[midday][i].province,
						value: this.everynum[midday][i].ratio
					}]
				}
				this.refresh();
				console.log(this.HFdata);
			},

			refresh() {
				let echarts = require('echarts');
				//let myChart = echarts.init(document.getElementById('container'),'dark');	
				let mapBoxEchart = echarts.init(document.getElementById('mapBox'), "dark");
				let geoCoordMap = {
					'新疆': [86.61, 40.79],
					'西藏': [89.13, 30.66],
					'黑龙江': [128.34, 47.05],
					'吉林': [126.32, 43.38],
					'辽宁': [123.42, 41.29],
					'内蒙古': [112.17, 42.81],
					'北京': [116.40, 40.40],
					'宁夏': [106.27, 36.76],
					'山西': [111.95, 37.65],
					'河北': [115.21, 38.44],
					'天津': [117.04, 39.52],
					'青海': [97.07, 35.62],
					'甘肃': [103.82, 36.05],
					'山东': [118.01, 36.37],
					'陕西': [108.94, 34.46],
					'河南': [113.46, 34.25],
					'安徽': [117.28, 31.86],
					'江苏': [120.26, 32.54],
					'上海': [121.46, 31.28],
					'四川': [103.36, 30.65],
					'湖北': [112.29, 30.98],
					'浙江': [120.15, 29.28],
					'重庆': [107.51, 29.63],
					'湖南': [112.08, 27.79],
					'江西': [115.89, 27.97],
					'贵州': [106.91, 26.67],
					'福建': [118.31, 26.07],
					'云南': [101.71, 24.84],
					'台湾': [121.01, 23.54],
					'广西': [108.67, 23.68],
					'广东': [113.98, 22.82],
					'海南': [110.03, 19.33],
				};

				let planePath = 'arrow'; // 箭头的svg
				// push进去线路开始-结束地点-经纬度
				let convertData = function(data) {
					let res = [];
					for (let i = 0; i < data.length; i++) {
						let dataItem = data[i];
						let fromCoord = geoCoordMap[dataItem[0].name];
						let toCoord = geoCoordMap[dataItem[1].name];
						if (fromCoord && toCoord) {
							res.push([{
								coord: fromCoord
							}, {
								coord: toCoord
							}]);
						}
					}
					return res;
				};

				let color = ['#aa0000', '#aa0000', '#ffff7f']; //圆圈和字的颜色，线的颜色，箭头颜色
				// 数据
				let series = [];
				// 遍历由合肥到其他城市的线路
				// console.log(this.HFdata);
				[
					['湖北', this.HFdata]
				].forEach(function(item, i) {
					// 配置
					series.push({
						// 系列名称，用于tooltip的显示
						name: item[0],
						type: 'lines',
						zlevel: 1, // 用于 Canvas 分层，不同zlevel值的图形会放置在不同的 Canvas 中
						// effect出发到目的地 的白色尾巴线条
						// 线特效的配置
						effect: {
							show: true,
							period: 6, // 特效动画的时间，单位为 s
							trailLength: 0.1, // 特效尾迹的长度。取从 0 到 1 的值，数值越大尾迹越长
							color: '#aa0000', // 移动箭头颜色
							symbol: planePath,
							symbolSize: 6 // 特效标记的大小
						},
						// lineStyle出发到目的地 的线条颜色
						lineStyle: {
							normal: {
								color: color[i],
								width: 0,
								curveness: 0.2 //幅度
							}
						},
						data: convertData(item[1]) //开始到结束数据
					}, {
						//出发地信息
						name: item[0],
						type: 'lines',
						zlevel: 2,
						effect: {
							show: true,
							period: 6,
							trailLength: 0,
							symbol: planePath,
							symbolSize: 6
						},
						lineStyle: {
							normal: {
								color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
									offset: 0,
									color: '#55ffff' // 出发
								}, {
									offset: 1,
									color: '#5555ff ' // 结束 颜色
								}], false),
								width: 1.5,
								opacity: 0.4,
								curveness: 0.2
							}
						},
						data: convertData(item[1])
					}, {
						// 目的地信息
						name: item[0],
						type: 'effectScatter',
						coordinateSystem: 'geo',
						zlevel: 2,
						rippleEffect: {
							brushType: 'stroke'
						},
						label: {
							normal: {
								show: true,
								position: 'right',
								formatter: '{b}'
							}
						},
						symbolSize: function(val) {
							return val[2] / 8;
						},
						itemStyle: {
							normal: {
								color: color[i]
							}
						},
						data: item[1].map(function(dataItem) {
							return {
								name: dataItem[1].name,
								value: geoCoordMap[dataItem[1].name].concat([dataItem[1].value] * 20)
							};
						})
					});
				});

				// 指定相关的配置项和数据
				var mapBoxOption = {
					geo: {
						map: 'china',
						roam: true, // 是否开启鼠标缩放和平移漫游。默认不开启。如果只想要开启缩放或者平移，可以设置成 'scale' 或者 'move'。设置成 true 为都开启
						aspectScale: 0.75,
						zoom: 1.20,
						label: {
							normal: {
								show: false,
								textStyle: {
									color: '#00a0c9'
								}
							},
							emphasis: { // 对应的鼠标悬浮效果
								show: false,
								textStyle: {
									color: "#00a0c9"
								}
							}
						},
						itemStyle: {
							normal: {
								areaColor: '#002134',
								borderColor: '#0066ba'
							},
							emphasis: {
								borderWidth: 0,
								borderColor: '#aa0000',
								areaColor: "#aa0000",
								shadowColor: 'rgba(0, 0, 0, 0.5)'
							}
						}
					},
					series: series
				};
				mapBoxEchart.setOption(mapBoxOption);
				window.addEventListener("resize", function() {
					mapBoxEchart.resize();
				});
			}
		},

		mounted() {
			this.refresh();
		},
	}
</script>
