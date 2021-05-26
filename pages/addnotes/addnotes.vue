<template>
<view>
	<!-- 选择图片 -->
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
			<view class="bg-img"  >
				<image :src='imagePath' ></image>
				<view class="cu-tag bg-red" catchtap="DelImg" >
					<text class="cuIcon-close"></text>
				</view>
			</view>
			<view class="solids">
				<text class="cuIcon-cameraadd"></text>
			</view>
		</view>
	</view>
	
	<!--标题-->
	<view class='cu-bar bg-white margin-top margin-bottom'>
	    <input name="title" placeholder-class="placeholder" placeholder="标题"  class="inputclass" v-model="mod_title"></input>
	</view>
	
	<!-- 输入框 -->
	<view class="cu-form-group margin-top yuyinWrap">
	  <textarea class="yuyinCon" placeholder='在这里输入您的想法吧' bindinput="textareaAInput" maxlength="-1" style="height: 400rpx;" v-model="mod_details"></textarea>
	  <button class="yuyinBtn " bindtouchstart="touchStart" bindtouchend="touchEnd">
	      <text>按住 说话</text>
	    </button>  
	</view>
	  	  <!-- 开始语音 弹出语音图标表示正在录音 -->
<!--	  <cover-view class="startYuyinImage" wx:if="{{recordState == true}}">
	    <view class="cu-item" wx:for="{{icon}}" wx:if="{{item.isShow}}">
	    		<text class="cuIcon-{{item.name}} lg text-gray" style="font-size:70px"></text>
	  	</view>
	    <cover-view>开始语音</cover-view>
	  </cover-view>
	</view>
-->	
	<view class="cu-bar btn-group leftbtn">
	    <button class="cu-btn bg-blue shadow-blur" @click="toSave()"
		 style="background: linear-gradient(45deg,#86abd5,#b8c9dd);margin-top: 80rpx;">保存
			</button>
	</view>
	
	<view class="cu-bar btn-group rightbtn">
	    <button class="cu-btn bg-red shadow-blur" @click="toDelete()"
		style="background: linear-gradient(135deg,#df9090,#f2b5b2);margin-top: 80rpx;">删除
			</button>
	</view>
</view>
</template>













<script>
	export default {
		data() {
			return {
				    userid:'',
				    mod_title:'',
				    mod_details:'',
				    imagePath:'',
				    imgList: [],
				    //语音
				    recordState: false, //录音状态
				    content:'',//内容
				    icon:[{ name: 'voicefill', isShow: true }]
			}
		},
		onLoad(e) {
			console.log("该条note的_id:",e.nid);
			this.getNote(e.nid);
		},
		methods:{
			getNote:function(nnid){
				console.log("是我nid",nnid)
				var self = this;
				
				this.userid = uni.getStorageSync('isCanUse');
				const db = wx.cloud.database({})
				db.collection('notes').where({
					_openid : self.userid
				}).orderBy('ltime','desc').get({
					success:function(res){
						console.log("成功获取该条note",res.data[nnid]);
						self.mod_title = res.data[nnid].title;
						self.mod_details = res.data[nnid].details;
						self.imagePath = res.data[nnid].imgList;
					}
				})
			},
			toDelete(){
				uni.showToast({
					title:'删除成功'
				})
			},
			toSave(){
				uni.showToast({
					title:'修改成功'
				})
			}
			
		},
		
	}
</script>

<style>
	@import "../../colorui/main.css";
	@import "../../colorui/icon.css";
	
	@import url("../../static/style/addnotes.css");
</style>
