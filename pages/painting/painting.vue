<template>
<<<<<<< HEAD
	
	
</template>

<script>
</script>

<style>
=======
	<view>
		<!-- 内容区 -->
		<view class="content">
			<!-- 左上日期 -->
			<view id="date">
				<view id="date_d" class="date_">{{date_d}}</view>
				<view id="line" class="date_"></view>
				<view id="date_y_m">
					<view id="date_m">{{date_m}}</view>
				</view>
			</view>
			<!-- 右上功能键 -->
			<view class="right-top">
				<view id="top_tool_1">
					<view class="t_tool iconfont icon-ico-back"></view>
				</view>
				<view id="top_tool_2">
					<view class="t_tool iconfont icon-ico-forward"></view>
				</view>
				<view id="top_tool_3">
					<view class="t_tool cuIcon-close" @click="clear()"></view>
				</view>
			</view>
			<!-- 画布 -->
			<canvas id="myCanvas" canvas-id="myCanvas" disable-scroll="true" @touchmove="tmove" @touchstart="tstart"
				@touchend="tend" @touchcancel="tcancel"></canvas>

			<!-- 悬浮工具 -->
			<view>
				<!-- <image id="tool_icon" src="../../static/images/tool.png" @click="showTools()"></image> -->

				<view id="tool_box">
					<view class="cuIcon-down a_tool_1" id="a_tool_1"></view>
					<view class="iconfont icon-baocuntupian a_tool_2" id="a_tool_1"></view>
					<view class="iconfont icon-xiangpi a_tool_3 " id="a_tool_1"></view>
					<view class="cuIcon-write a_tool_4" id="a_tool_1" @click="mPen()"></view>
					<view id="myPen" v-show="myPen_flag">
						<view id="myPen_color">
							<view class="myColor" v-for="i in colorArr" :key="i.color" :style="{background: i.color}"
								@click="updateColor(i.color)"></view>
						</view>
						<view id="myPen_thickness">
							<view class="myThick" v-for="j in thickness" :key="j.thickness"
								:style="{width: j.thickness/5+'rpx'}" @click="updateThick(j.thickness)"></view>
						</view>
						<view id="myPen_weight"></view>
					</view>
				</view>
			</view>
		</view>
		<!-- 自定义tabbar -->
		<view class="" v-if="show" @tap="switchDialog"></view>
		<Tabbar nowIndex="0" @switchDialog="switchDialog"></Tabbar>
	</view>
</template>

<script>
	//import Mycanvas from "../../static/js/theCanvas.js";

	var x = 0;
	var y = 0;
	import Tabbar from "../../components/tabbar/tabbar";
	export default {
		components: {
			Tabbar
		},
		data() {
			return {
				// 自定义tabbar页面展示
				show: false,
				// flag:false,
				myPen_flag: false,
				'date_d': '00',
				'date_y': '2020',
				'date_m': 'Aay',
				ctx: '',
				point: [],
				colorArr: [ //画笔颜色
					{
						color: "#fe4365",
						active: true
					},
					{
						color: "#f9cdad",
						active: false
					},
					{
						color: "#19CAAD",
						active: false
					},
					{
						color: "#a7dce0",
						active: false
					},
					{
						color: "#b3a8e9",
						active: false
					},
					{
						color: "#03263a",
						active: false
					}
				],
				thickness: [ //画笔粗细 
					{
						thickness: 10,
						active: false
					},
					{
						thickness: 20,
						active: false
					},
					{
						thickness: 30,
						active: true
					},
					{
						thickness: 40,
						active: false
					},
					{
						thickness: 50,
						active: false
					},
					{
						thickness: 60,
						active: false
					}
				],
			}
		},
		onLoad() {
			this.getTime(this);

			this.ctx = uni.createCanvasContext("myCanvas", this); //绘画对象

		},

		methods: {

			// 自定义页面
			switchDialog() {
				let bool = this.show;
				this.setData({
					show: !bool
				});
			},


			// showTools(){
			// 	this.flag = !this.flag;
			// },
			mPen() {
				this.myPen_flag = !this.myPen_flag;
			},
			//获取当前时间
			getTime() {
				let date = new Date();

				var y = date.getFullYear();

				var m = date.getMonth();
				switch (m) {
					case 0:
						m = '1';
						break;
					case 1:
						m = '2';
						break;
					case 2:
						m = '3';
						break;
					case 3:
						m = '4';
						break;
					case 4:
						m = '5';
						break;
					case 5:
						m = '6';
						break;
					case 6:
						m = '7';
						break;
					case 7:
						m = '8';
						break;
					case 8:
						m = '9';
						break;
					case 9:
						m = '10';
						break;
					case 10:
						m = '11';
						break;
					case 11:
						m = '12';
						break;
				}

				let d = date.getDate();
				d = d < 10 ? ('0' + d) : d; //天补0

				this.date_d = d;
				this.date_y = y;
				this.date_m = m;
			},

			//绘制功能
			//开始触摸，获取起点
			tstart(e) {
				let startX = e.changedTouches[0].x;
				let startY = e.changedTouches[0].y;
				let sPoint = {
					X: startX,
					Y: startY
				};
				this.point.push(sPoint);
				this.ctx.beginPath();
			},
			//移动手势
			tmove(e) {
				let moveX = e.changedTouches[0].x;
				let moveY = e.changedTouches[0].y;
				let movePoint = {
					X: moveX,
					Y: moveY
				};
				this.point.push(movePoint); //存点
				if (this.point.length >= 2) {
					this.draw(); //绘制路径
				}
			},
			//停止手势
			tend() {
				this.point = [];
			},
			//触摸取消

			//绘制
			draw: function() {
				let point1 = this.point[0]
				let point2 = this.point[1]
				this.point.shift()
				// console.log(JSON.stringify(touchs))
				this.ctx.moveTo(point1.X, point1.Y)
				this.ctx.lineTo(point2.X, point2.Y)
				this.ctx.stroke()
				this.ctx.draw(true);
			},
			//清除
			clear: function() {
				let that = this;
				uni.getSystemInfo({
					success: function(res) {
						let canvasw = res.windowWidth;
						let canvash = res.windowHeight;
						that.ctx.clearRect(0, 0, canvasw, canvash);
						that.ctx.draw(true);
					},
				})
			},
			//选择画笔颜色
			updateColor: function(color) {
				console.log(color);
				this.ctx.strokeStyle = color;
			},
			updateThick: function(thickness) {
				console.log(thickness);
				this.ctx.lineWidth = thickness / 10;
			}

		}
	}
</script>








<style>
	@import url("../../static/style/painting_style.css");

	@font-face {
		font-family: 'iconfont';
		src: url('data:font/ttf;charset=utf-8;base64,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') format('truetype');
		font-weight: 500;
		font-style: normal;
	}
>>>>>>> 7d72864 (ui设计)
</style>
