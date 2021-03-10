<script>
	
	export default {
	
		onLaunch: function() {
			//uni.clearStorage();//!!!!!!!!!!!!!!!!!!!!!!!!!!!
			uni.getStorage({
				key:"LaunchFlag",
				fail:function(){//第一次
					uni.reLaunch({
					        url: '/pages/guide/login'
					});
					uni.setStorage({
						key:"LaunchFlag",
						data:"1"
					})
				}
			});
			
			uni.getStorage({
				key:"AudioLanguage",
				fail:function(){
					uni.setStorage({
						key:"AudioLanguage",
						data:"普通话"
					});
				}
			});
			uni.getStorage({
				key:"City",
				fail:function(){
					uni.setStorage({
						key:"City",
						data:"成都"
					});
				}
			});
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
	/*每个页面公共css */
</style>
