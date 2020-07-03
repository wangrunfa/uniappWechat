<template>
	<view>

		<!-- <cu-custom bgColor="bg-gradual-blue" isBack="{{true}}">
		  <view slot="backText">主页</view>
		  <view slot="content">大全</view>
		</cu-custom> -->
		<view class="toptab" :style="{ height : iStatusBarHeight + 'px'}"></view>
		 <view  :style="{ height : iStatusBarHeight + 'px'}"> </view>
		<scroll-view scroll-y class="indexes" :scroll-into-view="'indexes'+listCurID" style="height:calc(100vh)"
		 scroll-with-animation="true" enable-back-to-top="true">
			<block v-for="(listDataKey,sub1) in listData">

				<!--  <view class="padding indexItem-{{listData[sub1]}}" id="indexes-{{listData[sub1]}}" >{{listData[sub1]}}{{listData[item]}}</view> -->
				<view class="padding font-color-200" :id="'indexes'+letterDataList[sub1]">{{letterDataList[sub1]}}</view>
				<view class="cu-list menu-avatar no-padding">
					<block v-for="(items,sub) in listDataKey.length">
						<view class="cu-item background-color-30 font-color-200" @click="nextSkip(listDataKey[sub].petHrefPath)">
							<view class="cu-avatar">
								<image :src="listDataKey[sub].petImgPath" mode="aspectFit"></image>
							</view>
							<view class="contents">
								<view class="text-grey">
									<text class="text-abc">{{listDataKey[sub].petName}}</text>
									
								</view>
								<block v-for='(englishName,englishIndex) in listDataKey[sub].petData[0]'>
									<text v-if='englishIndex === " 英文名" '>{{englishName}}</text>
								</block>
							</view>
						</view>
					</block>
				</view>
			</block>
		</scroll-view>
		<view class="indexBar" style="height:calc(100vh - 50px)">
			<view class="indexBar-box background-color-30 font-color-200" @touchstart="tStart" @touchend="tEnd" @touchmove="tMove">
				<view class="indexBar-item" v-for="(letterItem,letterIndex) in letterDataList" :id="letterIndex" @touchstart="getCur"
				 @touchend="setCur">{{letterItem}}</view>
			</view>
		</view>
		<!--选择显示-->
		<view :hidden="hidden" class="indexToast background-color-30 font-color-220">
			{{listCur}}
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
				<!-- <view class='bottom-button-box'>
					<view class='bottom-button-box2'  @click="searchSikp()">
						<view class='bottom-button-icon ttf-icon'>
							&#xe61c;
						</view>
						<view class='bottom-button-title'>搜索</view>
					</view>
				</view> -->
			<!-- 	<view class='bottom-button-box'>
					<view class='bottom-button-box2'>
						<view class='bottom-button-icon ttf-icon'>
							&#xe63a;
						</view>
						<view class='bottom-button-title'>上传</view>
					</view>
				</view> -->
			</view>
		
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				listCur: 'A',
				hidden: true,
				listData: [],
				letterDataList: [],
				listCurID: 0,
				listCur: 0,
				iStatusBarHeight:0,
			}
		},
		onLoad() {
			this.petListData();	
			this.iStatusBarHeight = uni.getSystemInfoSync().statusBarHeight
		 
		
		},
		methods: {
			petListData: function() {
				var that = this
				let list = [];
				for (let i = 0; i < 26; i++) {
					list[i] = String.fromCharCode(65 + i)
					console.log(list[i])
				}
				this.letterDataList = list

				// this.setData({
				//   list: list,
				//   listCur: list[0]
				// })
				// if (options.path==="/dog/"){
				//   this.setData({
				//     titlename:"萌犬"
				//   })
				// }
				// if (options.path === "/cat/") {
				//   this.setData({
				//     titlename: "喵咪"
				//   })
				// }
				uni.request({
					url:  this.websiteUrl+'/PetInfoData',
					data: {
						petHrefPath: '/cat/'
					},
					header: {
						'Content-Type': 'application/json'
					},
					method: 'GET',
					dataType: 'json',
					responseType: 'text',
					success: function(res) {

						console.log(res.data)

						that.listData = res.data

					},
					fail: function(res) {},
					complete: function(res) {},
				})
			},
			nextSkip: function(e) {
				console.log(e)
		
				uni.navigateTo({
					url: '/pages/pet/cat/pet-cat-info?path=' + e
				});
			},
			topSkip:function(){
				// if(){
					
				// }else{
					uni.navigateBack({
						delta:1,
						
					})
				// }
			
			},
			onReady() {
				let that = this;
				wx.createSelectorQuery().select('.indexBar-box').boundingClientRect(function(res) {

					that.boxTop = res.top

				}).exec();
				wx.createSelectorQuery().select('.indexes').boundingClientRect(function(res) {

					that.barTop = res.top

				}).exec()
			},
			//获取文字信息
			getCur(e) {

				this.hidden = false
				console.log(e)
				this.listCur = this.letterDataList[e.target.id]

			},

			setCur(e) {

				this.hidden = true,
					this.listCur = this.listCur

			},
			//滑动选择Item
			tMove(e) {
				let y = e.touches[0].clientY,
					offsettop = this.boxTop,
					that = this;
				//判断选择区域,只有在选择区才会生效
				if (y > offsettop) {
					let num = parseInt((y - offsettop) / 20);

					this.listCur = this.letterDataList[num]

				};
			},

			//触发全部开始选择
			tStart() {

				this.hidden = false

			},

			//触发结束选择
			tEnd() {

				this.hidden = true,
					this.listCurID = this.listCur

			},
			indexSelect(e) {
				let that = this;
				let barHeight = this.data.barHeight;
				let list = this.data.list;
				let scrollY = Math.ceil(list.length * e.detail.y / barHeight);
				for (let i = 0; i < list.length; i++) {
					if (scrollY < i + 1) {

						this.listCur = list[i],
							this.movableY = i * 20

						return false
					}
				}
			}
		}
	}
</script>

<style>
	
	@import "../../../conponents/bottom-nav.css";
	.indexes {
		position: relative;
	}

	.padding {
		padding: 10rpx 30rpx;
	}

	.indexBar {
		position: fixed;
		right: 0px;
		bottom: 0px;
		padding: 20rpx 20rpx 20rpx 60rpx;
		display: flex;
		align-items: center;
	}

	.cu-avatar {
		width: 125rpx;
		height: 125rpx;
		float: left;
		background-size: cover;
		border-radius: 50%;
		overflow: hidden;
		margin-left: 20rpx;
	}

	.cu-avatar>image {
		width: 125rpx;
		height: 125rpx;
		
	}

	.cu-list.menu-avatar>.cu-item {
		height: 140rpx;
		padding: 17rpx 10rpx 0;
		
		margin: 5px 10px;
		border-radius: 4px
	}

	.indexBar .indexBar-box {
		width: 40rpx;
		height: auto;
		
		display: flex;
		flex-direction: column;
		box-shadow: 0 0 20rpx rgba(0, 0, 0, 0.1);
		border-radius: 10rpx;
	}

	.text-grey {
		font-size: 30rpx
	}

	.contents {
		width: 510rpx;
		height: 96rpx;
		float: left;
		padding-left: 30rpx;
		
		line-height: 55rpx
	}

	.indexBar-item {
		flex: 1;
		width: 40rpx;
		height: 40rpx;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 24rpx;

	}

	movable-view.indexBar-item {
		width: 40rpx;
		height: 40rpx;
		z-index: 9;
		position: relative;
	}

	movable-view.indexBar-item::before {
		content: "";
		display: block;
		position: absolute;
		left: 0;
		top: 10rpx;
		height: 20rpx;
		width: 4rpx;
	
	}

	.indexToast {
		position: fixed;
		top: 0;
		right: 80rpx;
		bottom: 0;
	
		width: 100rpx;
		height: 100rpx;
		border-radius: 10rpx;
		margin: auto;
		
		line-height: 100rpx;
		text-align: center;
		font-size: 48rpx;
	}
</style>
