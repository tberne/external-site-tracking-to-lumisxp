<template>
	
</template>

<script>
import { lxpConfig } from "../lxp.config"
let startTime = -1
export default {
	components: {
	},
	mounted() {
		
		if( ! window.lum_track ) {
			window.lum_track=window.lum_track||function(){(lum_track.q=lum_track.q||[]).push([+new Date].concat([].slice.call(arguments)))};
			lum_track('config',{baseHref:lxpConfig.server});

			let elem = document.createElement("script")
			elem.src = lxpConfig.server + "/lumis/portal/monitor/script/track.js?_=" + new Date().getTime()
			document.body.appendChild(elem)
		}

		startTime = new Date().getTime()
		lum_track("event", "lumis.portal.monitor.ev.pageview")
	},
	beforeUnmount() {
		lum_track("event", "lumis.portal.monitor.ev.leavepage", {
			"lum_event.duration": new Date().getTime() - startTime
		})
	}
}
</script>

<style>

</style>