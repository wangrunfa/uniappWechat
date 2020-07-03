<script>
	export default {
	
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
@font-face {
  font-family: 'iconfont';  /* project id 1598223 */
  src: url('//at.alicdn.com/t/font_1598223_7tkmcazr233.eot');
  src: url('//at.alicdn.com/t/font_1598223_7tkmcazr233.eot?#iefix') format('embedded-opentype'),
  url('//at.alicdn.com/t/font_1598223_7tkmcazr233.woff2') format('woff2'),
  url('//at.alicdn.com/t/font_1598223_7tkmcazr233.woff') format('woff'),
  url('//at.alicdn.com/t/font_1598223_7tkmcazr233.ttf') format('truetype'),
  url('//at.alicdn.com/t/font_1598223_7tkmcazr233.svg#iconfont') format('svg');
}
	.ttf-icon {
	    font-family: iconfont;
	}
	
	/*每个页面公共css */
	 
	 .toptab{
		 width: 750rpx;
		 position: fixed;
		 top: 0;
		 background: #ffffff;
		 z-index: 1200;
	 }
	 page{
		 background: rgb(245,245,245);
	 }
	 
	 .font-color-255{
		 	 color: rgb(255,255,255);
	 }
	 
	 .font-color-220{
	 		 color: rgb(220,220,220);
	 }
	 
	 .font-color-200{
	 		  color: rgb(0,0,0);
	 }
	 .background-color-60{
		 background: rgb(240,240,240);
	 }
	 .background-color-40{
	 		 background: rgb(255,255,255);
	 }
	 .background-color-30{
	 		 background: rgb(255,255,255);
	 }
	 .background-color-1e1e1e{
	 		 background: #1E1E1E;
	 }
	 
</style>
