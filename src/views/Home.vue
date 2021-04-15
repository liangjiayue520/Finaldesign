<style scoped>
	#container {
		width: 100%;
		height: 50rem;
		border: 1px solid red;
		margin: 0 auto;
	}
</style>
<template>
	<div id="container">
	</div>
</template>

<script>
	export default {
		name: 'Home',
		data: function() {
			return {
				massage: "毕业设计",
				month: [],
				list: [],
				num: [],
				address: [],
				//dataMap: [],
			}
		},

		methods: {
			printlist() {
				//console.log(this.list);
				// console.log(this.month);
				console.log(this.num);
				console.log(this.month);
				// console.log(this.dataMap);
			},
			//为data：list赋值，获取完整数据。
			getdata() {
				return new Promise((resolve, reject) => {
					const axios = require('axios');
					axios.get("static/qaq.json").then((res) => {
						let arr = []; //存放日期的属性   
						let add = []; //存放中国31个省份名字
						let datamap = [];

						//获取日期属性 like:23-Jan,18-May
						for (let key in res.data[0]) {
							if (key == "" || key == "province" || key == "field1")
								continue;
							arr.push(key);
							this.month.push(key);
						}
						//console.log(arr) //获取成功


						//获取31个省份名字
						for (let i = 0; i <= 30; i++) {
							add.push(res.data[i].province);
							this.address.push(res.data[i].province);
						}
						//console.log(add); //获取成功


						//let everydayall = [];
						let flag = 0;
						for (let j = 0; j < 117; j++) {
							flag = arr[j];
							//这句话很重要，因为开始不理解js每次循环要新建对象，上一次的值会一直被覆盖，在js中往数组中push数据时，
							//总是被对象的最后一个值所覆盖，因为只有一个对象，但是每次映射都修改了对象的名字，只会保存最后一次，所以需要每次赋值/push时
							//都要new 或者 var 新对象
							let everyday = [];
							for (let i = 0; i < 31; i++) {
								for (let key in res.data[i]) {
									if (flag == key) {
										everyday[i] = {
											name: add[i],
											value: res.data[i][key]
										}
									}
								}
							}
							this.list.push(everyday);
						}


						for (let i = 0; i < 31; i++) {
							let par = arr[i];
							this.num.push(res.data[0][par]);
						}
						resolve();
					});
				})

			},

		},
		async mounted() {

			await this.getdata();

			let echarts = require('echarts');
			let myChart = echarts.init(document.getElementById('container'));

			let dataMap = [];
			//疫情人数地图数据value
			for (let i = 0; i < 31; i++) {
				dataMap[i] = {
					name: this.address[i],
					value: this.num[i]
				}
				//console.log(this.address[])
			}

			// for (let i = 0; i < 31; i++) {
			// 	console.log(this.address[i]);
			// }

			// console.log(this.address);


			//这段代码很重要 对dataMap进行处理，使其可以直接在页面上展示
			let specialMap = [];
			for (let i = 0; i < specialMap.length; i++) {
				for (let j = 0; j < dataMap.length; j++) {
					if (specialMap[i] == dataMap[j].name) {
						dataMap[j].selected = true;
						break;
					}
				}
			}

			let option = {
				baseOption: {
					title: {
						text: "全国疫情热力图",
						left: "45%",
						textStyle: {
							fontSize: 30,
							color:"#fff"
						},
					},
					timeline: {
						axisType: 'category',
						orient: 'vertical',
						autoPlay: true,
						inverse: true,
						playInterval: 1000,
						left: null,
						right: 0,
						top: 20,
						bottom: 20,
						width: 70,
						height: null,
						symbol: 'none',

						checkpointStyle: {
							borderWidth: 2
						},
						controlStyle: {
							showNextBtn: false,
							showPrevBtn: false
						},
						data: []
					},
					tooltip: {
						formatter: function(params) {
							var info = '<p style="font-size:18px">' + params.name + '</p><p style="font-size:14px">感染人数</p>' + params.value
							return info;
						},
						backgroundColor: "#e27081", //提示标签背景颜色
						textStyle: {
							color: "#ffffff"
						} //提示标签字体颜色
					},
					series: [{
						type: 'map',
						mapType: 'china',


					}],
					animationDurationUpdate: 1000,
					animationEasingUpdate: 'quinticInOut'
				},


				options: [

				]

			};

			for (var n = 0; n < 117; n++) {
				option.baseOption.timeline.data.push(this.month[n]);
				option.options.push({
					backgroundColor:"#000",
					visualMap: {
						show: true,
						x: 'left',
						y: 'center',
						textStyle: {
							color: 'white'
						},
						splitList: [{
								start: 60000,
								end: 70000
							}, {
								start: 70000,
								end: 80000
							},
							{
								start: 40000,
								end: 50000
							}, {
								start: 50000,
								end: 60000
							},
							{
								start: 10000,
								end: 20000
							}, {
								start: 20000,
								end: 40000
							},
							{
								start: 3000,
								end: 5000
							}, {
								start: 5000,
								end: 10000
							},
							{
								start: 1000,
								end: 2000
							}, {
								start: 2000,
								end: 3000
							},
							{
								start: 500,
								end: 700
							}, {
								start: 700,
								end: 1000
							},
							{
								start: 200,
								end: 400
							}, {
								start: 400,
								end: 500
							},
							{
								start: 0,
								end: 100
							}, {
								start: 100,
								end: 200
							},
						],
						color: ['#7f1100', '#ff5428', '#ff8c71', '#ffd768'],
						//color: ['#7f1100','#ff5428','#ff8c71' ] 
					},
					series: {
						type: 'map',
						mapType: 'china',
						zoom: 1, //当前视角的缩放比例
						roam: true, //是否开启平游或缩放
						scaleLimit: { //滚轮缩放的极限控制
							min: 1,
						},
						data: this.list[n],
						label: {
							normal: {
								show: true, //显示省份标签
								// font - size: 
								borderColor: '#0066ba'
							},
							emphasis: {
								borderColor: '#ffff00',
								areaColor: "#aa0000",
								show: true, //对应的鼠标悬浮效果
							}
						},
					}
				});
			}

			myChart.setOption(option);

			// 建议加上以下这一行代码， 不加的效果图如下（ 当浏览器窗口缩小的时候）
			window.addEventListener('resize', function() {
				myChart.resize()
			});
		},
	}
</script>
