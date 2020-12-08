<script>
	import { mapState } from 'vuex'
    export default {
        onLaunch: function() {
            console.log('App Launch');
        },
        onShow: function() {
            console.log('App Show')
        },
        onHide: function() {
            console.log('App Hide')
        },
		globalData: {
			test: ''
		}
    }
</script>

<style>
</style>
