<template>
	<view class="">
		<view class="logo flex" open-type="getUserInfo" @tap="getUserInfo" :hover-class="!login ? 'logo-hover' : ''">
			<image class="backg_img" src="../../static/bgimage/bg.png"></image>
			<image class="logo-img " :src="avatarUrl"></image>
			<view class="logo-title">
				<text class="uer-name" >Hi，{{nickName}}</text>
				<!-- <text class="go-login navigat-arrow" v-if="!login">&#xe65e;</text> -->
			</view>
		</view>
		<view class="login_text" @tap="getUserInfo">{{nickName}}</view>
		<!-- 列表 -->
		<view class="cu-list menu card-menu margin-top">
			<view class="cu-item arrow" @click="help" >
				<view class="content flex">
					<view class="cuIcon-form"></view>
					<text class="margin-left">使用帮助</text>
				</view>
			</view>
			<view class="cu-item arrow" @click="licence">
				<view class="content flex">
					<view class="cuIcon-form"></view>
					<text class="margin-left">隐私协议</text>
				</view>
			</view>
		</view>
		<view class="cu-list menu card-menu margin-top">
			<view class="cu-item arrow" @click="about">
				<view class="content flex">
					<view class="cuIcon-form"></view>
					<text class="margin-left">关于</text>
				</view>
			</view>
			<view class="cu-item arrow" @click="feedback">
				<view class="content flex">
					<view class="cuIcon-form"></view>
					<text class="margin-left">意见反馈</text>
				</view>
			</view>
		</view>
		<!-- 自定义tabbar -->
		<view v-if="show" @tap="switchDialog"></view>
		<Tabbar nowIndex="2" @switchDialog="switchDialog"></Tabbar>
	</view>
</template>













<script>
	import Tabbar from "../../components/tabbar/tabbar";
	export default {
		components: {
			Tabbar
		},
		data() {
			return {
				// 自定义tabbar页面展示
				show: false,
				login: false,
				uerInfo: [],
				code: '',
				OpenId: '',
				nickName: '点击登录',
				avatarUrl: "../../static/images/default.png",
				isCanUse: ''
			}
		},
		onLoad() {
			this.getUserInfo();
		},
		methods: {
			// 自定义页面
			switchDialog() {
				let bool = this.show;
				this.setData({
					show: !bool
				});
			},
				//微信授权登录
			            getUserInfo() { // 获取用户信息
						const db = wx.cloud.database({}) //配置云数据库
						var that = this
							wx.login({
								success: (res) => {
									that.code = res.code
									console.log("获取code",res.code);
								}
							})
							uni.getUserProfile({
								desc: '登录',
								success: function(res) {
									console.log("成功 用户信息",res.userInfo);
									that.nickName = res.userInfo.nickName;  //获取昵称
									that.avatarUrl = res.userInfo.avatarUrl;  //获取头像
									uni.getStorageSync('isCanUse',res.userInfo._openid);
									
									uni.showToast({
										title:'登录成功'
									})
									
									db.collection('user').add({  //存入云数据库
									             data:{									 
									               username:res.userInfo.nickName,
									               useravatarurl : res.userInfo.avatarUrl,
									               usergender : res.userInfo.gender
									             }
									           })
									db.collection('user').where({
										username : that.nickName
									}).get({
										success:function(re){
											console.log(re.data[0]._openid);
										    uni.setStorageSync('isCanUse',re.data[0]._openid);
										}
									})	   
								},
								fail: (res) => {
									console.log(res)
								}
							});
						},
			//登录

			feedback() {
				uni.navigateTo({
					url: '/pages/user/feedback'
				})
			},
			licence() {
				uni.navigateTo({
					url: '/pages/user/licence'
				})
			},
			help() {
				uni.navigateTo({
					url: '/pages/user/help'
				})
			},
			about() {
				uni.navigateTo({
					url: '/pages/user/about'
				})
			}
		}
	}
</script>













<style>
	@import "../../colorui/main.css";
	@import "../../colorui/icon.css";

	@import url("../../static/style/user.css");
</style>
