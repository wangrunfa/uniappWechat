<template>
	<view class="pagebackgroundAll" >
		<view class="toptab" :style="{ height : iStatusBarHeight + 'px'}"> </view>
		 <view  :style="{ height : iStatusBarHeight + 'px'}"> </view>
		  <view class="page-section-spacing">
		                     <swiper class="slideshow-box" indicator-dots="true" autoplay="true" interval="3000" duration="700" circular="true" >
		                         <swiper-item>
		                             <view class="swiper-item uni-bg-red">A</view>
		                         </swiper-item>
		                         <swiper-item>
		                             <view class="swiper-item uni-bg-green">B</view>
		                         </swiper-item>
		                         <swiper-item>
		                             <view class="swiper-item uni-bg-blue">C</view>
		                         </swiper-item>
		                     </swiper>
		                 </view>
		 
		 
		  <view class="titles">表情包:</view>
		<view class='nav-maxBox'>
		   <view class='nav-maxBox-left' bindtap="mapPage">
			   <view class='nav-maxBox-left-button' @click="expressionSkip()">
				     <image class="nav-maxBox-left-button-image" src='https://cedar8.cn/wechatImages/yls1.png'></image>
			   </view>
		   
		   </view>
		   <view class='nav-maxBox-right'>
		      <view class='nav-maxBox-right-button-01' data-pethref="/dog/" bindtap='petListTap'><image src='/static/sucai/gougou.png'></image></view>
		      <view class='nav-maxBox-right-button-01' data-pethref="/cat/" bindtap="petListTap"><image src='/static/sucai/maomi.png'></image></view>
		   </view>
		</view>
		 
		 
		    <view class='email-titles' bindtap="partyGame">聚会小游戏</view>
		<view class='email-titles'>开发者邮箱：cedar6@qq.com</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				iStatusBarHeight:0,
				 background: ['color1', 'color2', 'color3'],
				           
			}
		},
		onLoad() {
			this.iStatusBarHeight = uni.getSystemInfoSync().statusBarHeight
		 
		},
		
		methods: {
			expressionSkip:function(){
			        uni.navigateTo({
			              url: '../phizpackage/phizpackage'
			          });
				}
		}
	}
</script>

<style>
	
	    @font-face {
	        font-family: 'iconfont';
	        src: url('https://at.alicdn.com/t/font_865816_17gjspmmrkti.ttf') format('truetype');
	    }
	    .test {
	        font-family: iconfont;
	        margin-left: 20rpx;
	    }

	.slideshow-box{
		width: 750rpx;
		height: 350rpx;
		
	}
.nav-maxBox{
  height: 410rpx;
  width: 710rpx;
  margin: 0 auto;
  
}
.nav-maxBox-left{
width: 355rpx;
height: 400rpx;
float: left;
}


.shadow-blur {
	position: relative;
}
.nav-maxBox-right{
width: 355rpx;
height: 400rpx;
float: left;
}
.nav-maxBox-left-button{
  width: 345rpx;
  height: 370rpx;
  margin: 20rpx 0;
  border-radius: 7px;
 background:#333333;
 overflow: hidden;
}
.nav-maxBox-left-button-image{
	width: 345rpx;
	height: 370rpx;
}
.nav-maxBox-right-button-01{
 width: 345rpx;
  height: 175rpx;
  margin: 20rpx 0 0 10rpx;
  border-radius: 7px;
 background:rgba(255, 255, 255);
 overflow: hidden
}
.nav-maxBox-right-button-01>image{
   width: 345rpx;
  height: 175rpx;
}
.image-group-box {
  width: 730rpx;
  margin: 10rpx auto 0;
  display: flex;
  white-space: nowrap;
}
.titles{
  height: 30px;
  text-indent: 30rpx;
  font-size: 15px;
  color: white;
  font-weight: bold;
  border-left: 3px solid blue;
  line-height: 30px;
  background: #1E1E1E;
  
}
.email-titles{
  height: 50px;
  width: 710rpx;
  margin: 20rpx;
  color: rgb(238, 186, 42);
  text-indent: 30rpx;
  font-size: 17px;
  line-height: 50px;
  background: rgb(255, 255, 255);
  text-align: center;
  border-radius: 7px;
  font-weight: bold;
  size: 15px;

}

</style>
