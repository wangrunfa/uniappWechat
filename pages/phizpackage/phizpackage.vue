<template>
	<view class="pagebackgroundAll">
		<view class="topberbox">
			<view class="toptab" :style="{ height : iStatusBarHeight + 'px'}">
				<view :style="{width: progress}"></view>
			</view>
			 <view  :style="{ height : iStatusBarHeight + 'px'}"> </view>
			 
			<view class="navigationBar background-color-30 font-color-200">
				<scroll-view class='show-top-classify' scroll-x="true">
					<block v-for="(item,index) in tabs" :key="item.spell">
						<view v-if="item.spell == classifys" style="border-bottom:3px solid rgb(200,200,200);" @click="switchover($event,item.spell)">{{item.name}}</view>
						<view v-else @click="switchover($event,item.spell)">{{item.name}}</view>
					</block>
				</scroll-view>
			</view>
			<view class="download-progress-box">
				<view  class="download-progress-bar" :style="{width: progress}"></view>
			</view>
		</view>
        <view  :style="{height: iStatusBarHeight+50 + 'px'}"></view>
	
		<view class="history_content background-color-60">
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
		<view class="bottom-to-load">
			<view class="load-image-date" style="left: 325rpx;">
				<view class="load-image-date-animation">
					<view class="loader">
						<view class="line-scale-pulse-out-rapid">
							<view></view>
							<view></view>
							<view></view>
							<view></view>
							<view></view>
						</view>
					</view>
				</view>
			</view>
			
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
					<view class='bottom-button-box2'  @click="searchSikp()">
						<view class='bottom-button-icon ttf-icon'>
							&#xe61c;
						</view>
						<view class='bottom-button-title'>搜索</view>
					</view>
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
	import history from "../../conponents/history.vue";
	var startNumbers = 0;
	var stopNumbers = 20;
	var switchs = true;
	var tripsLeftHeight = 0;
	var tripsReightHeight = 0;
    var windowWightHalf=(wx.getSystemInfoSync().windowWidth/2)

	export default {

		data() {
			return {
				progress: '0%',
				tripsLeft: [],
				tripsReight: [],
				classifys: "all",
				start: 0,
				loading: false,
				iStatusBarHeight: 0,
				tabs: [{
					name: "全部",
					spell: "all"
				}, {
					name: "GIF图",
					spell: "GIFimg"
				}, {
					name: "时间排序",
					spell: "timerank"
				}, ]


			}
		},
		//加载事件
		onLoad() {
			this.iStatusBarHeight = uni.getSystemInfoSync().statusBarHeight
			uni.startPullDownRefresh({});
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
			searchSikp:function(){
				uni.navigateTo({
				    url: '../search/search'
				});
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
					if (e.detail.x < windowWightHalf && currentIndex == i) {
						this.tripsLeft[currentIndex].testDisabled = 1;
						break;
					}
					if (e.detail.x > windowWightHalf && currentIndex == i) {
						this.tripsReight[currentIndex].testDisabled = 1;
                          break;
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
							url: 'https://www.cedar8.cn:8443/EP/downloadsPlusOne',
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
							if (e.detail.x < windowWightHalf && currentIndex == i) {
								this.tripsLeft[currentIndex].testDisabled = 2;
								this.tripsLeft[currentIndex].downloads = this.tripsLeft[currentIndex].downloads + 1;
							}
							if (e.detail.x > windowWightHalf && currentIndex == i) {
								this.tripsReight[currentIndex].testDisabled = 2;
								this.tripsReight[currentIndex].downloads = this.tripsReight[currentIndex].downloads + 1;

							}
						}

						this.progress = '0%'

					}
				})

			},
			
			findList: function() {
				var that = this;
				uni.request({
					url: 'https://www.cedar8.cn:8443/EP/findList',

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

@import "../../conponents/bottom-nav.css";
	.topberbox {
		z-index: 1002;
	
		position: fixed;
	}
.download-progress-box{
	height: 4px;width: 750rpx
	}
.download-progress-bar{
	height: 4rpx;
	 background-image: linear-gradient(to bottom right, red, yellow);
}
	.load-image-date {
		position: relative;
		width: 115rpx;
		height: 0;
		left: 132rpx;
	}

 .line-scale-pulse-out-rapid>view{
	 background: black;
 }

	.navigationBar {
		height: 44px;
		width: 750rpx;
	}

	/**index.wxss**/
/* 
	.show-top-one {
		width: 750rpx;
		box-shadow: 0 2px 5px 0 #1f1f1f;
		position: fixed;
		z-index: 1001;
		top: 0;
	} */

/* 
	.navtopss {
		width: 100%;
	} */

	.show-top-classify {
		width: 550rpx;
		margin-right: 20rpx;
		margin-top: 11px;
		float: left;
		white-space: nowrap;
		display: flex;
		position: fixed;
		font-size: 15px;
		font-weight: bold;
	}

	.show-top-classify view {
		height: 30px;
		padding: 0 20rpx;
		line-height: 25px;
		width: auto;
		display: inline-block;
	}

/* 	.show-top-two {
		width: 750rpx;
		height: 300rpx;
	}
 */
	.history_content {
		clear: both;
		overflow: hidden;
		width: 100%;
		z-index: 202;
	}
	.bottom-to-load{
		width: 750rpx;
		height: 150rpx;
		
	}	
	</style>
