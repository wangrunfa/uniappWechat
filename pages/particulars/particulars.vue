<template>
	<view class="pagebackgroundAll">

		<!-- 头部 -->
		<view class="barheight" :style="{ height : iStatusBarHeight +'px'}"></view>

		<view class='top-box' :style="{ top : iStatusBarHeight +'px'}">
			<view class='top-return'>
				<view class='top-return2'>
					<view style='border-right:1px #e2ebe7 solid' bindtap='index_skip'>
						<image src='../../static/icon/1.png'></image>
					</view>
				</view>
			</view>
			<view class='top-name'>表情包详情</view>
		</view>

		<!-- 轮播图 -->
		<view class="max-image-box">
			<image class="max-image-image" mode="aspectFit" src="https://www.cedar8.cn/stuphoto/E5AE405813C711EA8AE5701CE732AC7A.jpg"></image>
		</view>
		<text class="ImageName">爱你呦，迷迷的</text>

		<scroll-view class="max-image-group-box" scroll-x="true">
			<view class="labels">
				迷迷的
			</view>
			<view class="labels">
				迷迷的
			</view>
			<view class="labels">
				迷迷的
			</view>
		</scroll-view>
		<view class="subheading">2019:8:5</view>
		<view class="subheading">推荐表情包</view>
		<view class="history_content">
			<scroll-view scroll-y="true" scroll-with-animation='true' scroll-top='50' bindscrolltolower="loadMore"
			 upper-threshold>
				<view class="left">
					<m-history :datalist="tripsLeft"></m-history>

				</view>
				<view class="right">
					<m-history :datalist="tripsReight"></m-history>

				</view>
			</scroll-view>

		</view>


		<!-- 底部导航 -->
		<view class='bottom-box'>
			<view class='bottom-box2'>
				<view class='bottom-button-box'>
					<view class='bottom-button-box2'>
						<view class='bottom-button-icon'>
							<image src='../../static/icon/WX.png'></image>
						</view>
						<view class='bottom-button-title'>分享</view>
					</view>
				</view>
				<view class='bottom-button-box'>
					<view class='bottom-button-box2'>
						<view class='bottom-button-icon'>
							<image src='../../static/icon/JC.png'></image>
						</view>
						<view class='bottom-button-title'>举报</view>
					</view>
				</view>
				<view class='bottom-button-box'>
					<view class='bottom-button-box2'>
						<view class='bottom-button-icon'>
							<image src='../../static/icon/DH.png'></image>
						</view>
						<view class='bottom-button-title'>下载</view>
					</view>
				</view>
			</view>

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
				progress: '0%',
				tripsLeft: [],
				tripsReight: [],
				classifys: "all",
				start: 0,
				loading: false,
				iStatusBarHeight: 0
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
							uni.showToast({
								title: '已到世界尽头~',
							})
						}
					},
					fail(res) {
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
	/* pages/introduce/introduce.wxss */
	.subheading {
		height: 35px;
		width: 700rpx;
		background: white;
		border-radius: 10px;
		text-align: center;
		line-height: 35px;
		font-weight: bold;
		margin: 20rpx 25rpx;
	}

	.barheight {
		position: fixed;
		top: 0;
		width: 750rpx;
		background: white;
		z-index: 1000;
	}

	.top-box {
		width: 750rpx;
		height: 44px;
		position: fixed;
		background: white;
		z-index: 1000;
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

	.top-return2>view>image {
		width: 22px;
		height: 22px;
		margin-left: 8px;

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

	.max-image-box {
		width: 500rpx;
		height: 500rpx;
		margin: 0 125rpx 20rpx;
		padding-top: 150rpx;

	}

	.max-image-image {
		width: 500rpx;
		height: 500rpx;
		background: none;
	}

	.ImageName {
		text-align: center;
		width: 750rpx;
		display: block;
		font-size: 22px;
		font-weight: bold;
	}

	.load-image-date {
		position: relative;
		top: 20px;
		width: 100rpx;
		height: 0;
		left: 132rpx;
	}

	.load-image-date-animation {
		width: 100rpx;
		height: 100rpx;
	}

	.bottom-box {
		width: 750rpx;
		height: 100rpx;
		position: fixed;
		bottom: 0px;
		background: white;
		z-index: 1000;
	}

	.bottom-box2 {
		width: 750rpx;
		height: 60rpx;
		margin-top: 20rpx;
	}

	.bottom-button-box {
		width: 250rpx;
		height: 60rpx;
		float: left;
	}

	.image-group-box {
		width: 100%;
		height: 50rpx;
		overflow: hidden;
	}

	.max-image-group-box {
		width: 500rpx;
		height: 50rpx;
		overflow: hidden;
		margin: 20rpx 125rpx;
	}

	.labels {
		padding: 0px 5px;
		display: inline-block;
		height: 50rpx;
		float: left;
		background: #000000;
		display: inline-block;
		margin: 0 3px 0 0;
		line-height: 50rpx;
		border-radius: 3px;
		color: rgb(253, 253, 252);
	}

	.bottom-button-box2 {
		width: 160rpx;
		height: 60rpx;
		margin: 0 auto;
		border-radius: 35px;
		background: #000000;
		padding: 0px 10px;
		color: white
	}

	.bottom-button-icon {
		width: 60rpx;
		height: 60rpx;
		float: left
	}

	.bottom-button-title {
		width: 100rpx;
		height: 60rpx;
		float: left;
		text-align: center;
		line-height: 58rpx;
		font-size: 34rpx;

	}

	.bottom-button-icon>image {
		width: 60rpx;
		height: 60rpx;
	}




</style>
