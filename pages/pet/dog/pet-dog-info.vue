<template>
	<view>
		<view class="toptab" :style="{ height : iStatusBarHeight + 'px'}"></view>
		 <view  :style="{ height : iStatusBarHeight + 'px'}"> </view>
		<view class="top-box">
		  <view class="headImages">
		    <view class="headImages-2">
		      <image :src="listData.petImgPath"></image>
		    </view>
		  </view>
		</view>
		<view class="petname font-color-255">{{listData.petName}}</view>
		<view class="title font-color-200 background-color-30">详情信息</view>
		<view class="particulars  font-color-200 background-color-30">
		  <view class="particulars-view" v-for="(dataitem1,dataindex1) in listData.petData[0]">
		    <view class="particulars-view-key">
		      <text>{{dataindex1}}:</text>
		    </view>
		    <view class="particulars-view-item">{{dataitem1}}</view>
		  </view>
		</view>
		<view class="title font-color-200 background-color-30">特征百分比</view>
		<view class="particulars  font-color-200 background-color-30">
		  <view class="particulars-view" v-for="(dataitem2,dataindex2) in listData.petAttribute[0]">
		    <view class="characteristic-key1 background-668B8B">
		      <view class="characteristic-key2 background-BA55D3" :style="{width:dataitem2}">
		        <view class="characteristic-key3">{{dataindex2}}</view>
				 <view class="characteristic-key4">{{dataitem2}}</view>
		      </view>
		    </view>
		  </view>
		</view>
		<view class="title font-color-200 background-color-30">档案</view>
		<view class="particulars  font-color-200 background-color-30">
		  {{listData.petRecord}}
		</view>
		<view class="title font-color-200 background-color-30">可爱图</view>
		<view class="particulars font-color-200 background-color-30" style="margin-bottom: 120rpx;padding-bottom: 30rpx;">
		  <block v-for="(dataitem3,dataindex3) in listData.petShowGather">
		  <view  id="lovelinessimages">
		    <image  :src="dataitem3" mode="widthFix"></image>
		    </view>
		  </block>
		</view>
		
		<!-- 底部导航 -->
		<view class='bottom-box'>
			<view class='bottom-box2'>
				<view class='bottom-button-box'>
					<view class='bottom-button-box2' @click="topSkip()">
						<view class='bottom-button-icon ttf-icon'>
							&#xe619;
						</view>
						<view class='bottom-button-title'>返回</view>
					</view>
				</view>
				<view class='bottom-button-box'>
					
				</view>
				<view class='bottom-button-box'>
					<view class='bottom-button-box2'>
						<view class='bottom-button-icon ttf-icon'>
							&#xe63a;
						</view>
						<view class='bottom-button-title'>上传</view>
					</view>
				</view>
			</view>
		
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				listData:[],
				iStatusBarHeight:0,
			}
		},
		  onLoad(options) {
			  	this.iStatusBarHeight = uni.getSystemInfoSync().statusBarHeight
			  var this_s=this
		    uni.request({
		      url: 'https://www.cedar8.cn/petInfo',
		      data: {
		        petHrefPath: options.path
		      },
		      header: { 'Content-Type': 'application/json' },
		      method: 'GET',
		      dataType: 'json',
		      responseType: 'text',
		      success: function(res) {
		        console.log(res.data[0])
		          this_s.listData = res.data[0]
		      },
		      fail: function(res) {},
		      complete: function(res) {},
		    })
		  },
		methods: {
			topSkip:function(){
				
				if(getCurrentPages().length>1){
					uni.navigateBack({
						delta:1
					})
				}else{
					uni.redirectTo({
						url:"pet-cat-list"
					})
				}
				
			}
		}
	}
</script>

<style>
	
	@import "../../../conponents/bottom-nav.css";
/* pages/doginfo/doginfo.wxss */

.top-box{
  height: 350rpx;
  width: 750rpx;
  
}
.background-668B8B{
	background:#668B8B;
}
.background-BA55D3{
	background: #FFB90F;
	color: #FFFAF0;
}
.title{
  width:710rpx;
  height: 55rpx;
  margin:  0 20rpx;
  line-height: 55rpx;
   float: left;
  text-align: center;
   border-radius:20rpx; 
  overflow: hidden;}
.headImages{
  width: 750rpx;
  height: 350rpx;
  float: left;
}
.headImages-2{
  width: 250rpx;
  height: 250rpx;
  margin: 50rpx auto;
  border-radius: 20rpx;
  overflow: hidden
}
.headImages-2>image{
   width: 250rpx;
  height: 250rpx;
}
.petname{
   float: left;
  width: 750rpx;
  height: 100rpx;
  line-height: 100rpx;
  text-align: center;
  font-size: 20px;
  
}
.particulars{
  float: left;
  width:670rpx;
  margin: 20rpx 20rpx;
  
  padding: 20rpx;
  border-radius:20rpx; 
  min-height:50rpx;
  overflow: hidden;
  
}
.particulars-view{
  height: 50rpx;
  width: 100%;
  float: left;
  border-radius:20rpx; 
  overflow: hidden;
  margin-bottom: 15rpx;
}
.particulars-view-key{
  padding: 10rpx 30rpx;
  min-width: 160rpx;
  float: left
}
.particulars-view-key>text{
  padding: 0 10rpx;
}
.particulars-view-item{
  padding: 10rpx 0;
   float: left
}
.characteristic-key1{
  width: 100%;
  height: 50rpx;
}
.characteristic-key2{
  height: 50rpx;
  border-radius: 5rpx;
  
}
.characteristic-key3{
 float: left;
   padding: 0 5rpx;
   width:200rpx;
   height: 50rpx;
   line-height: 50rpx;
   padding-left: 15rpx;
  }
  .characteristic-key4{
     text-align: right;
	 float: right;
     padding: 0 5rpx;
     width:200rpx;
     height: 50rpx;
     line-height: 50rpx;
     padding-left: 15rpx;
    }
#lovelinessimages{
    width: 300rpx;
    height: 300rpx;
    margin:30rpx 0 0 25rpx;
    border-radius: 6px;
    overflow: hidden;
    float: left;
  }
#lovelinessimages>image{
   width: 300rpx;
  display: block
}
</style>
