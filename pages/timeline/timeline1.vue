<template>
<view class="history_content">
	<view class="history_content_item" v-for="(item,id) in historyList" :key="id">
		<view class="history_right_item" >
			<view class="item_year">{{item.rightYear}}</view>
			<view class="item_line">
				<view class="item_line_dot"></view>
			</view>
			<view class="item_desc">
				<view class="desc_list" v-for="(right,id) in item.rightItem" :key="id">
					<view class="title">{{right.rightTitle}}</view>
					<view class="word">{{right.rightDesc}}</view>
				</view>
			</view>  	
		</view>
	</view>
	
	<view id="theButt">
		<view id="theTop" class="butt">up</view>
		<view id="theBottom" class="butt">down</view>
	</view>
</view>
</template> 

<script>
	export default{
			data:function(){
			return {
				historyList: [
					{
						id: "0",
						leftYear: '2020年',
						rightYear: "2019年",
						leftItem: [
							{
								id: 0,
								leftTitle: "5月",
								leftDesc: '文案文案文案文案文案文案文案文案文案文案文案文案文案'
							},{
								id: 1,
								leftTitle: "4月",
								leftDesc: '文案文案文案文案文案文案文案文案文案文案文案文案文案'
							}
						],
						rightItem: [
							{
								id: 0,
								rightTitle: "12月",
								rightDesc: '文案文案文案文案文案文案文案文案文案文案文案文案文案'
							},{
								id: 1,
								rightTitle: "9月",
								rightDesc: '文案文案文案文案文案文案文案文案文案文案文案文案文案'
							}
						]
					},	
					{
						id: "1",
						leftYear: '2018年创新',
						rightYear: "2017年",
						leftItem: [
							{
								id: 0,
								leftTitle: "12月",
								leftDesc: '文案文案文案文案文案文案文案文案文案文案文案文案文案'
							},{
								id: 1,
								leftTitle: "11月",
								leftDesc: '文案文案文案文案文案文案文案文案文案文案文案文案文案'
							}					
						],
						rightItem: [
							{
								id: 0,
								rightTitle: "12月",
								rightDesc: '文案文案文案文案文案文案文案文案文案文案文案文案文案'
							},{
								id: 1,
								rightTitle: "10月",
								rightDesc: '文案文案文案文案文案文案文案文案文案文案文案文案文案'
							}
						]
					},
				]
			}
		}

/*	export default{
		data:function(){
			return{
				
				dayList:[],
				colors:['bg-gradual-red','bg-gradual-orange','bg-gradual-green','bg-gradual-blue','bg-gradual-purple','bg-gradual-pink'],
				memorys:{}
			}
		}
		methods:{
			navigator:function(){
				this.$emit('itemClick')
			},
	        to_today:function(item){
	        	// yyyy--mm--dd
	        	uni.setStorageSync('today',this.util.format(item.date.getTime(),true))
	        	// 转跳详情页vue
	        	// uni.redirectTo({ 
	        	// 	url:'/pages/../..'
	        	// })
				
	        },
			// 获取日历
			get_dayList:function(){
				let that = this;
				let start = new Date();
				
				// if(this.dayList.length !=0){
				// 	start = new Date(this.dayList[0].date.getTime() - 24*60*60*1000);
				// }
				for(let i = 0;i < 5;i++){
					// let temp = new Date(start.getTime() - i*24*60*60*1000);
					let ts = {
						// dt:this.util.format(temp.getTime(),true),
						// date:temp,
						year:start.getFullYear(),
						month:start.getMonth()+1,
						day:start.getDate()-i,
						// week:this.util.getShortWeek(temp.getDay()),
						random1:Math.floor(Math.random()*6)
					}
					console.log(ts)
					that.dayList.unshift(ts);
				}
				console.log(that.dayList)
			return;
				// 用户已登录 请求日历列表
				if(uni.getStorageSync('user')){
					uni.request({
						url:this.host +'/today/getMemoryList.html',
						method:'POST',
						header:this.util.header(),
						data:{
							userId:uni.getStorageSync('user').id,
							lang:uni.getStorageSync('lang'),
							startSearchDate:new Date(this.dayList[0].date.getTime() - 24*60*60*1000),
							endSearchDate:start,
							rows:5
						},
						success:(res)=>{
							if(res.data.flag ==0){
								res.data.beanList.forEach((item) => {
									this.$set(this.memorys,item.dt,item);
								});
							}else{
								this.util.toast(res.data.msg);
							}
						}
					}
					)
				}
			},
			// 查看图片大图
			view_head_image:function(){
				this.util.to_view_image(0,[img])
			},
			// 加载进入页面最底部
			to_bottom:function(){
				this.$nextTick(() =>{
					// 数据绑定后计算元素高度
					let contentDiv = uni.createSelectorQuery().select("#contentDiv");
					contentDiv.boundingClientRect((res) =>{
						uni.pageScrollTo({
							duration:500,
							scrollTop:res.height
						})
					}).exec();			
				})			
			}
		},
	    onLoad:function(){
			// 加载当天日期
			this.get_dayList();
			this.to_bottom();
		},
		onShow:function(){
			// 页面展示时获取语言
			this.lang = uni.getStorageSync('lang');

		},
		onPullDownRefresh:function(){
			this.get_dayList();
			uni.stopPullDownRefresh();
		}   
		*/
	}
</script>

<style>
	@import url("../../static/style/timeline.css");
</style>
