<template>
<view>
	<!-- 显示时间 -->
	<view class="time">{{today}}</view>
	
	<form @submit="formSubmit(userNote)" v-model="userNote">
		
	<!-- 添加图片 -->
	<view class="cu-bar bg-white">
			<view class="action">
				添加符合您心情的图片
			</view>
			<view class="action">
				1/9
			</view>
	</view>
	<view class="cu-form-group">
		<view class="grid col-4 grid-square flex-sub">
			<view class="grid col-4 grid-square flex-sub">
			    <view class="bg-img"  >	
				<image :src='imagePath' ></image>
			    	<view class="cu-tag bg-red" catchtap="DelImg" >	
						<text class="cuIcon-close"></text> 
			    	</view>
					
			    </view>
				<view class="solids" @click="ChooseImage()" >
				    <text class="cuIcon-cameraadd"></text>
			    </view>
			</view>

		</view>
	</view>
	
	<!-- 标题 -->
	<view class='cu-bar bg-white margin-top margin-bottom'>
	    <input v-model="userNote.title" placeholder-class="placeholder" placeholder="标题"  class="inputclass" />
	</view>
	
	<!-- 语音输入 -->
	<view class="cu-form-group margin-top yuyinWrap">
	  <textarea v-model="userNote.details" placeholder='在这里输入您的想法吧' bindinput="textareaAInput" maxlength="-1" style="height: 400rpx;"></textarea>
	  <button class="yuyinBtn " bindtouchstart="touchStart" bindtouchend="touchEnd" style="background: rgba(0, 0, 0, 0.3);">
	      <text >按住 说话</text>
	    </button>  
	</view>
	  <!-- 开始语音 弹出语音图标表示正在录音 -->
<!--	  <cover-view class="startYuyinImage" v-if="{{recordState == true}}">
			<cover-image src="../images/yuyin.png"></cover-image>
	    <cover-view>开始语音</cover-view>
	  </cover-view>
	</view>
-->	
	<!-- 保存按钮 -->
	<view class="cu-bar btn-group">
	    <button class="cu-btn bg-blue shadow-blur" id="sub_btn" form-type="submit" 
		style="background: linear-gradient(45deg,#D1587F,#5B4CB1,#F6C542);margin: 80rpx auto;">保存
			</button>
	</view>
	
	</form>	
</view>
</template>












<script>
	export default {
		data() {
			return {
				    userid:'',
				    userNote: {
				        title:'',
				        details:''
				    },
					today: new Date().toISOString().slice(0, 10),
				    imagePath:'',
				    imgp:'',
				    //语音
				    recordState: false, //录音状态
				    content:'',//内容
				    icon:[{ name: 'voicefill', isShow: true }]
			}
		},
		onLoad(){
			
		},
		methods: {
		 /*   formSubmit(e){
		        console.log(e)
		    },  */
			
			ChooseImage:function(){  //选择图片
				let that = this;
				wx.chooseImage({
					count:1,
					sizeType: ['original', 'compressed'],
					sourceType: ['album', 'camera'],
					success(res) {
					        console.log("图片选择成功",res);
							that.imagePath = res.tempFilePaths[0];
							wx.cloud.uploadFile({
							cloudPath:'user_img/'+new Date().getTime()+'.png', // 上传至云端的路径
							filePath: res.tempFilePaths[0], 
							 success: res2 => { 
								 console.log("云存储图片路径",res2.fileID);
								 that.imgp = res2.fileID;
							 },
							 fail:res3 =>{
								 console.log("上传失败",res3.errMsg);
								
							 }
						})  
					}
				})
			},
			
		 formSubmit:function(e){
				 console.log("获取页面输入成功",e);
				 var that = this;
				 
				if(uni.getStorageSync('isCanUse')){  //判断登录
					
					that.userid = uni.getStorageSync('isCanUse');
					console.log(that.userid);
					
					const db = wx.cloud.database({})
   
				    
						db.collection('notes').add({
							data:{
						
								title: that.userNote.title,
								details: that.userNote.details,
								imgList: that.imgp,
								ltime: db.serverDate()
							}
							}).then(res2=>{
                                uni.showToast({
                                	title:'保存成功！',
                                
                                })
                             })
                    
				    
				}else{
					uni.showToast({
						title:'尚未登录，无法保存内容',
						icon: 'none',    //如果要纯文本，不要icon，将值设为'none'
						duration: 3000 
					})
				}
			}  
				
		}
		 
	}
</script>

<style>
	@import "../../colorui/main.css";
	@import "../../colorui/icon.css";
	
	@import url("../../static/style/notes.css");
</style>
