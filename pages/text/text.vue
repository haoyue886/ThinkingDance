<template>
	<view class="content">
		<view v-for="(item,i) in hhh" :key="i">
		   <view id="text1">{{item.transcript}}</view>
		</view>
		<view>{{len}}</view>
	</view>
</template>

<script>
export default {
	data(){
		return{
			hhh: [],
			len:'1'
			
		}
	},	
	onLoad(){
		this.getList();

	},
	methods:{
		getList:function(){
			var self = this
			const db = wx.cloud.database({})
			db.collection('record').where({
			    _openid:'12345',
			  })
			  .get({
				  success:function(res){
					  console.log("成功",res.data);
					  self.hhh = res.data;
					  self.len = res.data.length;
				  },
			  });
			
			}
		
		
	}
}
</script>

<style>
	#text1{
		background-color: #007AFF;
		width: 500rpx;
		height: 500rpx;
	}
	
</style>
