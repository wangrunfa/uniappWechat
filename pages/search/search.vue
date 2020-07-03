<template>
	<view class="pagebackgroundAll">

		<!-- 头部 -->
		<view class="toptab" :style="{ height : iStatusBarHeight + 'px'}"> </view>
		 <view  :style="{ height : iStatusBarHeight + 'px'}"> </view>

		<view class='top-box background-color-30' :style="{ top : iStatusBarHeight +'px'}">
			<!-- 搜索框 -->
			<view class="searchbutton background-color-60" >
				<input type="text" :placeholder="placeholdertitle" class="searchInput" placeholder-style="color:white"></input>
				<image src="https://www.cedar8.cn/wechatImages/search.png"></image>
			</view>
			
		</view>
		<view class="contentbox" :style="{top:iStatusBarHeight+50+'px'}">
			<view class="searchListbox  background-color-40">
				<view class="searchList" v-for="(item,index) in tripsLeft">
					<view class="search-List-left-box">
						<image class="search-List-left-image" mode="aspectFit" :src="'https://www.cedar8.cn/stuphoto/'+item.randomId"></image>
					</view>
					<view class="search-List-centre-box">
						<view class="search-List-right-title font-color-200">{{item.title}}</view>
						<scroll-view scroll-x="true" scroll-with-animation='true' class="search-List-right-label" scroll-left="120">
							<view class="search-List-right-label-self">
							<block v-for="(itemlabel,indexlabel) in item.label">
								<view class="labelminbox font-color-220 background-color-30">{{itemlabel}}</view>

							</block>
							</view>
						</scroll-view>
						<view class="search-List-right-time font-color-220">{{item.originalTime}}</view>
						 <text class="search-List-right-three-point font-color-220 ttf-icon">&#xec1c;</text>
					</view>
					<view class="search-List-right-box">

						<view class="search-List-right-operation background-color-60">
							<view class="search-List-right-operation-download">
								<view class="search-List-right-operation-download-icon font-color-220  ttf-icon">&#xe7ed;</view>
							</view>
							<view class="search-List-right-operation-details">
								<view class="search-List-right-operation-download-icon font-color-220  ttf-icon">&#xe604;</view>
							</view>
						</view>
					</view>
				</view>
				<view class="bottom-to-load">
					<block v-if="loadstatus==0">加载中</block>
					<block v-if="loadstatus==1">加载失败，稍后再试</block>
					<block v-if="loadstatus==2">内容到底了</block>
					<!-- <view class="load-image-date" style="left: 325rpx;">
								<view class="line-scale-pulse-out-rapid">
									<view></view>
									<view></view>
									<view></view>
									<view></view>
									<view></view>
								</view>
					</view> -->
					
				</view>
			</view>

		</view>

<!-- 底部导航 -->
		<view class='search-bottom-box'>
			<view @click='topSkip()' class="search-return-bottom ttf-icon">&#xe619;</view>
			</view>
	</view>
</template>

<script>
	import history from "../../conponents/history.vue";
	var startNumbers = 0;
	var stopNumbers = 20;
	var switchs = true;
	var tripsLeftHeight = 0;
	var tripsReightHeight = 0;

	export default {

		data() {
			return {
				placeholdertitle: '请输入关键词！',
				progress: '0%',
				tripsLeft: [],
				tripsReight: [],
				classifys: "all",
				start: 0,
				loading: false,
				iStatusBarHeight: 0,
				loadstatus: 1,
			}
		},
		onLoad() {
			this.iStatusBarHeight = uni.getSystemInfoSync().statusBarHeight
			this.findList();
			
		},
		//下拉事件
		onPullDownRefresh() {
			this.tripsLeft = [];
			this.tripsReight = [];
			this.findList();
			console.log("asdasdasdasdsadsa")
			setTimeout(function() {
				uni.stopPullDownRefresh();
			}, 1000)
		},
		//上拉事件
		onReachBottom() {
			this.findList();
		},
		methods: {
			topSkip:function(){
				uni.navigateBack({
					delta:1
				})
			},
			switchover: function(e, name) {
				this.classifys = name;
				uni.startPullDownRefresh({});
			},

			labelSearchingBindtap: function(e) {
				console.log(e.currentTarget.dataset.labelsearching)
			},
			clickDownloadImage: function(e, currentIndex, path, idExpression) {
				console.log(e)
				console.log(path)
				console.log(currentIndex)
				console.log(idExpression)
				var array = this.tripsLeft;
				console.log(array)
				for (var i = 0; i < array.length; i++) {

					if (e.detail.x < 200 && currentIndex == i) {
						this.tripsLeft[currentIndex].testDisabled = 1;
						this.tripsLeft[currentIndex].downloads = this.tripsLeft[currentIndex].downloads + 1;
					}
					if (e.detail.x > 200 && currentIndex == i) {
						this.tripsReight[currentIndex].testDisabled = 1;
						this.tripsReight[currentIndex].downloads = this.tripsReight[currentIndex].downloads + 1;

					}

				}

				const downloadTask = uni.downloadFile({
					url: path, //仅为示例，并非真实的资源
					success(res) {
						uni.saveImageToPhotosAlbum({
							filePath: res.tempFilePath,
							success() {
								console.log("调用系统相册成功！")
							},
							fail() {
								console.log("调用系统相册失败！")
							}
						})
						if (res.statusCode === 200) {
							console.log('下载成功');
						}
					},
					fail(res) {
						console.log("错误")
					}
				})

				downloadTask.onProgressUpdate((res) => {

					console.log("downloadTask.onProgressUpdate--进入监听")
					if (res.progress < 100) {
						this.progress = res.progress + '%'
					} else {

						uni.request({
							url:  this.websiteUrl+'/EP/downloadsPlusOne',
							method: 'GET',
							headers: {
								'Content-Type': 'application/json'
							},
							data: {
								idExpressions: idExpression
							},


						})
						var array = this.tripsLeft;
						for (var i = 0; i < array.length; i++) {
							if (e.detail.x < 200 && currentIndex == i) {
								this.tripsLeft[currentIndex].testDisabled = 2;
								this.tripsLeft[currentIndex].downloads = this.tripsLeft[currentIndex].downloads + 1;
							}
							if (e.detail.x > 200 && currentIndex == i) {
								this.tripsReight[currentIndex].testDisabled = 2;
								this.tripsReight[currentIndex].downloads = this.tripsReight[currentIndex].downloads + 1;

							}
						}

						this.progress = res.progress + '%'

					}
				})

			},
			findList: function() {
				var that = this;
				uni.request({
					url:  this.websiteUrl+'/EP/findList',

					headers: {
						'Content-Type': 'application/json'
					},
					data: {
						startNumber: startNumbers,
						stopNumber: stopNumbers,
						classify: this.classifys
					},
					success: function(res) {
						console.log(res.data)
						if (switchs) {
							that.loadstatus=0
							let addListLeft = that.tripsLeft;
							let addListReight = that.tripsReight;
							for (let i = 0; i < stopNumbers; i++) {
								if (res.data[i] == null && switchs) {
									switchs = false
								} else if (switchs) {

									if (tripsLeftHeight <= tripsReightHeight) {
										addListLeft.push(res.data[i])
										tripsLeftHeight += res.data[i].imageHeight;
									} else {
										addListReight.push(res.data[i])
										tripsReightHeight += res.data[i].imageHeight
									}
									if ((stopNumbers - 1) == i) {
										that.tripsLeft = addListLeft

										that.tripsReight = addListReight,

											console.log(addListLeft)
										console.log(addListReight)
										uni.stopPullDownRefresh();
									}
								}
							}
							startNumbers = startNumbers + stopNumbers
						} else {
							that.loadstatus=2
							uni.showToast({
								
								title: '已到世界尽头~',
							})
						}
					},
					fail(res) {
						that.loadstatus=0
						console.log("wx.request回调失败")
					},
					complete(res) {
						console.log("wx.request结束")
					}
				})
			}

		},
		components: {
			"m-history": history
		}
	}
</script>

<style>
	@import "../../conponents/history.css"; 	
	
	@import "search-bottom.css"; 	
	
	/* pages/introduce/introduce.wxss */
	/* 搜索列表 */
	

	
	
	.search-List-left-box {
		width: 200rpx;
		height: 200rpx;
		float: left;
	}

	.search-List-centre-box {
		width: 340rpx;
		height: 200rpx;
		float: left;
		margin: 0 15rpx;
	}

	.search-List-right-box {
		width: 100rpx;
		height: 200rpx;
		float: right;
	}

	.search-List-right-title {
		width: 320rpx;
		height: 40rpx;
		padding: 15rpx;
		
		float: left;
		font-size: 32rpx;
		font-weight: bold;
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
	}

	.search-List-right-label {
		width: 350rpx;
		height: 70rpx;
	
		float: left;
	}
.search-List-right-label-self{
	white-space: nowrap;
	height: 70rpx;
	min-width: 100rpx;
}
	.labelminbox {
		padding: 10rpx 15rpx;
		float: left;
		font-size: 30rpx;
		border-radius: 10rpx;
		margin: 5px 0 5px 15rpx;
		width: auto;
	}

	.search-List-right-operation {
		width: 100rpx;
		height: 200rpx;
		float: left;
	}

	.search-List-right-operation-download {
		width: 100rpx;
		height: 99rpx;
		border-bottom: 1px solid #ccc;
		float: left;

	}

	.search-List-right-operation-download-icon {
		width: 60rpx;
		height: 60rpx;
		margin: 20rpx;
		font-size: 50rpx;
		text-align: center;
		line-height: ;
		height: 60rpx;
	}


	.search-List-right-time {
		width: 270rpx;
		height: 40rpx;
		padding: 15rpx 0 0 15rpx;
		float: left;
	}
.search-List-right-three-point{
	width: 40rpx;
	height: 40rpx;
	float: right;
	padding: 15rpx 0 0 15rpx;
	line-height: 40rpx;
	font-size: 30rpx;
}
	.search-List-right-operation-details {
		width: 100rpx;
		height: 100rpx;
		float: left;
	}
	.search-List-left-image {
		width: 200rpx;
		height: 200rpx;
	}

	.searchListbox {
		width: 710rpx;
		margin:  0 20rpx;
		padding: 1rpx 0;
		border-radius: 16rpx;
		z-index: 999;
	}

	.searchList {
		width: 680rpx;
		height: 200rpx;
		margin: 15rpx;
		border: 1px solid rgba(150, 150, 150, 0.6);
		border-radius: 16rpx;
		overflow: hidden;
	}
 

	.top-box {
		width: 750rpx;
		height: 44px;
		z-index: 1000;
		position: fixed;
	}

	.top-return {
		height: 44px;
		width: 230rpx;
		float: left
	}

	.top-return2 {
		height: 32px;
		width: 30px;
		margin: 6px 10px;
		border-radius: 16px;

	}
	

	.top-return2>view {
		width: 39px;
		height: 22px;
		margin-top: 5px;
		float: left
	}

	 .left-top-return-button{
		line-height: 23px;
	 	font-size: 32px;
	 }

	.top-name {
		font-size: 15px;
		height: 44px;
		width: 280rpx;
		float: left;
		text-align: center;
		line-height: 44px;
		font-weight: bold
	}



	.contentbox {
		position: absolute;
	}

	.pullarrive {
		width: 750px;
		height: 100px;
	}

	/* 搜索 */
	.searchbutton {
	    margin-left: 15rpx;
		width: 500rpx;
		height: 40px;
		z-index: 10000;
		border-radius: 40rpx;
		 
	}


	.searchbutton>image {
		width: 40px;
		height: 40px;
		float: right;
		margin-right: 8px;
	}

	.searchInput {
		width: 60%;
		height: 40px;
		float: left;
		color: white;
		padding: 0 25rpx;
	}
.bottom-to-load{
		width: 750rpx;
		height: 150rpx;
	}
	
	.load-image-date {
		position: relative;
		width: 110rpx;
		height: 0;
		left: 132rpx;
	}
</style>
