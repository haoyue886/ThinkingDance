<template>
<view id="contentDiv">
	<view id="top_text">
		你一共记录了 {{len}} 条灵感。
	</view>
		<view class="cu-timeline" v-for="(item,index) in dayList" :key="index" >
			<view class="cu-time" >
			<text>{{item.ltime.toISOString().slice(0, 10)}}</text>
			</view>
			<view class="cu-item cur cuIcon-time" @click="gotoAddnotes(index)">
				<view class="content light shadow-blur" >
					<text>{{item.details}}</text>
				</view>
			</view>
		</view>
</view>
</template>










<script>
	export default{
		data:function(){
			return{
				userid:'',
				dayList:[],
				memorys:{},
				len:0
			}
		},
		onLoad() {
			this.get_list();
		},
		methods:{
			gotoAddnotes:function(e){
				var that = this;
				that.noteid = e;
				var nid = JSON.stringify(that.noteid);
				uni.navigateTo({
					url:'../addnotes/addnotes?nid='+nid
				})
			},
			get_list:function(){
				if(uni.getStorageSync('isCanUse')){
					this.userid = uni.getStorageSync('isCanUse');
					console.log(this.userid);
				}else{
					uni.showToast({
						title:'尚未登录，无法查看你的时间轴',
						icon: 'none',    //如果要纯文本，不要icon，将值设为'none'
						duration: 3000 
					})
				}
				
				var self = this
				const db = wx.cloud.database({})
				db.collection('notes').where({
					_openid : self.userid
				}).orderBy('ltime','desc').get({
					success:function(res){
						console.log("成功获取用户时间轴列表",res.data);
						self.dayList = res.data;
						self.len = res.data.length;
					}
				})
			},
			
/*				// 用户已登录 请求日历列表
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
			},  */
	/*		// 查看图片大图
			view_head_image:function(){
				this.util.to_view_image(0,[img])
			},*/
			// 加载进入页面最底部
	/*	to_bottom:function(){
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
			}   */
		},   
		onPullDownRefresh:function(){
			uni.stopPullDownRefresh();
		}
	}
</script>















<style>
	    @import "../../colorui/main.css";
	    @import "../../colorui/icon.css";	
		@import url("../../static/style/timeline.css");
	
	
</style>
