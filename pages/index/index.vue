<template>
	<view class="pagebackgroundAll" >
		<view class="toptab" :style="{ height : iStatusBarHeight + 'px'}"> </view>
		 <view  :style="{ height : iStatusBarHeight + 'px'}"> </view>
		  <view class="page-section-spacing">
		                     <swiper class="slideshow-box" indicator-color="rgba(0,229,238,1)" indicator-active-color="#e9981c" indicator-dots="true" autoplay="true" interval="3000" duration="700" circular="true" >
		                         <swiper-item v-for="(item,index) in shufflingFigure">
		                             <view class="swiper-item uni-bg-red swiperMaxBox">
										 <image class="swiperImage" :src="item.BGImage"></image>
										 <view class="swiperTitle">
											 <view class="swiperMaxTitle">{{item.swiperMaxTitle}}</view>
											 <view class="swiperMinTitle">{{item.swiperMinTitle}}</view>
									     </view>
									</view>
		                         </swiper-item>
		                        
		                     </swiper>
		                 </view>
		 
		 
		
		<view class='nav-maxBox'>
		   <view class='nav-maxBox-left' bindtap="mapPage">
			   <view class='nav-maxBox-left-button' @click="expressionSkip()">
				     <image class="nav-maxBox-left-button-image" src='https://cedar8.cn/wechatImages/yls1.png'></image>
			   </view>
		   
		   </view>
		   <view class='nav-maxBox-right'>
		      <view class='nav-maxBox-right-button-01' @click='petSkipCat()'><image src='/static/sucai/gougou.png'></image></view>
		      <view class='nav-maxBox-right-button-01' @click='petSkipDog()' bindtap="petListTap"><image src='/static/sucai/maomi.png'></image></view>
		   </view>
		</view>
		 
		 
		    <view class='email-titles' bindtap="partyGame">聚会小游戏</view>
		<view class='email-titles'>开发者邮箱：cedar6@qq.com</view>
<button @click="opensss">打开弹窗</button>
<uni-popup ref="popup" type="bottom">
	<div class="popups-style-box"></div>
</uni-popup>
	</view>
</template>

<script>
	import uniPopup from "@/components/uni-popup/uni-popup.vue"

	export default {
		
		data() {
			return {
				
				iStatusBarHeight:0,
				 background: ['color1', 'color2', 'color3'],
				 shufflingFigure:[
					 {BGImage:'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1591300385009&di=b8cb0ee846406de1d370c09603e1076c&imgtype=0&src=http%3A%2F%2Fimg.dahe.cn%2F2017%2F01-25%2F108207856.jpg',
					  swiperMaxTitle:'陪你度过漫长岁月',
					  swiperMinTitle:'陪伴是 最长情的告白'},
				     {BGImage:'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1591300266942&di=f5b1dae1de36145bbed59b4347a1e3a4&imgtype=0&src=http%3A%2F%2F5b0988e595225.cdn.sohucs.com%2Fimages%2F20180825%2F8f7df298ad604b36be2dfddc910a5a4c.jpeg',
					  swiperMaxTitle:'年少有为',
					  swiperMinTitle:'假如我年少有为-不自卑'},
				     {BGImage:'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1591300182267&di=1ee3a256a699803ca9675f3e5decd513&imgtype=0&src=http%3A%2F%2F5b0988e595225.cdn.sohucs.com%2Fimages%2F20190529%2Fc6a3924d26c049188d18063cf8fcff85.jpeg',
					  swiperMaxTitle:'致自己',
					  swiperMinTitle:'人生就像一场旅行'},
				 ]
			}
		},
		 components: {uniPopup},
		 
		onLoad() {
			this.iStatusBarHeight = uni.getSystemInfoSync().statusBarHeight
		 
		},
		
		methods: {
			 opensss:function(){
			        // 需要在 popup 组件，指定 ref 为 popup
			         this.$refs.popup.open()
			      },
			expressionSkip:function(){
			        uni.navigateTo({
			              url: '../phizpackage/phizpackage'
			          });
				},
				petSkipCat:function(){
				        uni.navigateTo({
				              url: '../pet/cat/pet-cat-list'
				          });
					},
					petSkipDog:function(){
					        uni.navigateTo({
					              url: '../pet/dog/pet-dog-list'
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
		.swiperMaxBox{
			position: relative;
		}
		.swiperImage{
			width: 710rpx;
			margin: 0 20rpx;
			border-radius: 20rpx;
			height: 710rpx;
			position: absolute;
			z-index: 60;
		}
		.swiperTitle{
				position: absolute;
				width: 650rpx;
				height: 170rpx;
			background: rgba(0,0,0,0.2);
				top: 500rpx;
				left: 50rpx;
				border-radius: 30rpx;
				z-index: 120;
				

		}
		.swiperMaxTitle,.swiperMinTitle{
			color: #ffffff;
			position: absolute;
			font-weight: bold;
		}
		.swiperMaxTitle{
			
			right: 15rpx;
			top:15rpx;
			font-size: 60rpx;
		}
		.swiperMinTitle{
		
			top: 100rpx;
			left: 30px;
			font-size: 35rpx;
		}
	
	
		.popups-style-box{
			height: 500rpx;
			width: 710rpx;
			margin: 0 20rpx;
			border-top-left-radius:30rpx ;
			border-top-right-radius:30rpx ;
			
			background: #fff;
		}

	.slideshow-box{
		width: 750rpx;
		height: 710rpx;
		
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
 background:white;
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

  font-weight: bold;
  border-left: 3px solid blue;
  line-height: 30px;

  
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
