<template name="template02">
	<view>
		<view v-for="(trip,index) in datalist">
			<view class='image-quantity background-color-60 font-color-200'>{{trip.downloads}}</view>
			<view class="history_item background-color-40">
				<view class="load-image-date" :style="{top:trip.imageHeight*132+'rpx'}">
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
				<image class="item-img" @click="popupsIncident($event,index,trip.idExpression)" :style="{height:trip.imageHeight*365+'rpx'}" :src="'https://www.cedar8.cn/stuphoto/'+trip.randomId"
				 mode="widthFix" item></image>
				<view class="item-title-box">
					<view url="url" class="item-title font-color-200">{{trip.title}}</view>
				</view>
				<view class="history_auth_name_buttom">
					<view class="hanb_left">
						<view class="hanb_left_biaoqian">
							
								<view class="labels background-color-30 font-color-220" v-for="(item2,index2) in trip.label" :key="">
									<view @click="labelSearchingBindtap(item2)">
										{{item2}}
									</view>
								</view>
						
						</view>
						<view class="hanb_left_time font-color-220">{{trip.originalTime}}</view>
					</view>

					<view v-if="trip.testDisabled==0">
						<view class="hanb_right font-color-220 download-icon ttf-icon" @click="childMethod($event,index,'https://www.cedar8.cn/stuphoto/'+trip.randomId,trip.idExpression)">
						   &#xe7ed;
						</view>
					</view>

					<view v-if="trip.testDisabled==1">
						<view class="hanb_right">
							<view class="maxBox">
								<view class="donhuohe">
									<view></view>
									<view></view>
									<view></view>
									<view></view>
									<view></view>
									<view></view>
									<view></view>
									<view></view>
									<view></view>
								</view>
							</view>
						</view>
					</view>

					<view v-if="trip.testDisabled==2">
						<view class="hanb_right font-color-220 download-icon ttf-icon">
							&#xe676;
						</view>
					</view>
				</view>

			</view>

		</view>
	</view>
</template>

<script>
	export default {
		name: "template02",
		//属性
		props: {
			datalist: {
				type: Array, //属性类型
				value: []
			},

		},
		//组件生命周期
		created: function(e) {

		},
		data() {
			return {

			}
		},

		methods: {
			childMethod(e, index, path, idExpression) {
				console.log("asdsadsadsa")
				this.$parent.clickDownloadImage(e, index, path, idExpression);
			}
			,
			popupsIncident(e, index, idExpression){
				console.log("asdsadsadsa")
				this.$parent.popupsIncidentMain(e, index, idExpression);
			}

		}
	}
</script>

<style>

</style>
