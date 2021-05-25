<template>
	<view class="">
		<view class="logo flex"  open-type = "getUserInfo" @getuserinfo = "getUserInfo" :hover-class="!login ? 'logo-hover' : ''">
			<image class="backg_img" src="../../static/bgimage/bg.png"></image>
			<image class="logo-img " :src="avatarUrl"></image>
			<view class="logo-title"  >
				<text class="uer-name" >Hi，{{nickName}}</text>
				<!-- <text class="go-login navigat-arrow" v-if="!login">&#xe65e;</text> -->
			</view>
		</view>
		<view class="login_text">点击登录</view>
		<!-- 列表 -->
		<view class="cu-list menu card-menu margin-top">
			<view class="cu-item arrow">
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
			<view class="cu-item arrow">
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
		<view  v-if="show" @tap="switchDialog"></view>
		<Tabbar nowIndex="2" @switchDialog="switchDialog"></Tabbar>
	</view>
</template>













<script>
	import Tabbar from "../../components/tabbar/tabbar";
	export default {
		components:{
			Tabbar
		},
		data() {
			return {
				// 自定义tabbar页面展示
				show: false,
				login: false,
				uerInfo: [],
				code:'',
				 OpenId: '',
				nickName: '请点击登录',
				avatarUrl: "../../static/images/default.png",
				isCanUse: uni.getStorageSync('isCanUse')||true//默认为true
			}
		},
		onLoad(){
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
		 getUserInfo() {
			 
			 const db = wx.cloud.database({}) //配置云数据库
			 
		                let _this = this;
		                uni.getUserInfo({
		                    provider: 'weixin',   //选择微信环境
		                    success: function(infoRes) {
								console.log("成功 用户信息",infoRes.userInfo);
		                        _this.nickName = infoRes.userInfo.nickName;  //获取昵称
								_this.avatarUrl = infoRes.userInfo.avatarUrl;  //获取头像
								 db.collection('user').add({  //存入云数据库
								              data:{
												 
								                username:infoRes.userInfo.nickName,
								                useravatarurl : infoRes.userInfo.avatarUrl,
								                usergender : infoRes.userInfo.gender
								              }
								            })
		                        try {
		                            uni.setStorageSync('isCanUse', false);//记录是否第一次授权  false:表示不是第一次授权
		                        } catch (e) {
									console.log("错误",e);
								}
		                    },
		                    fail(res) {
								console.log("错误",res); 
							}
		                });
		            },
		　　　　　　//登录

			feedback(){
				uni.navigateTo({url:'/pages/user/feedback'})
			},
			licence(){
				uni.navigateTo({url:'/pages/user/licence'})
			}
		}
	}
</script>













<style>
	@import "../../colorui/main.css";
	@import "../../colorui/icon.css";
	
	@import url("../../static/style/user.css");
 


</style>