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
			<button  class="cu-btn bg-blue"@click="choose_image">上传图片</button>	
		</view>
		<!-- 如果选择图片 进行展示 -->
		<view class="padding-sm text-center" v-if="bean.headImage">
			<image src="bean.headImage" mode="aspectFit"></image>
		</view>
		<view class="cu-bar tabbar bg-default foot">
			<view class="bg-orange submit" @click="save_bean">确认反馈</view>
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
