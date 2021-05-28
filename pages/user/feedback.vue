<template>
	<!-- 意见反馈页 -->
	<view>
		<view class="cu-form-group">
			<view class="title">反馈标题</view>
			<input type="text" v-model="bean.title" placeholder="请输入标题" />	
		</view>
		<view class="padding-sm margin-top bg-white">
			<textarea  placeholder="请输入反馈内容" v-model="bean.cotent" />
		</view>
		<view class="cu-form-group margin-top">
			<view class="title">反馈图片</view>
			<button  class="cu-btn bg-blue"@click="ChooseImage()">上传图片</button>	
		</view>
		<!-- 如果选择图片 进行展示 -->
		<view class="padding-sm text-center" v-if="bean.headImage">
			<image src="bean.headImage" mode="aspectFit"></image>
		</view>
		<view class="cu-bar tabbar bg-default foot">
			<view class="bg-purple submit" @click="save_bean">确认反馈</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				bean:{}
			}
		},
		methods: {
			// 上传图片
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
			choose_image(){
			
			},
			// 保存方法
			save_bean(){}
		}
	}
</script>

<style>
	@import "../../colorui/main.css";
	@import "../../colorui/icon.css";
	
	page{
		background-color: #f4f8f9;
	}
</style>
