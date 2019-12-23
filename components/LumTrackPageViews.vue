<template>
	
</template>

<script>
import { lxpConfig } from "../lxp.config"

let lastPageviewTime = new Date().getTime()

let collectLeavePage = ( url ) => {
	let duration = new Date().getTime() - lastPageviewTime

	let payload = {
		"lum_event.duration": duration,
		"lum_request.mode.id": "0"
	}

	if ( url ) {
		payload["lum_client.url"] = url
	}

	lum_track("event", "lumis.portal.monitor.ev.leavepage", payload)
}

let collectPageView = () => {
	lum_track("event", "lumis.portal.monitor.ev.pageview", {
		"lum_request.mode.id": "0"
	})

	lastPageviewTime = new Date().getTime()
}

let onUnloadCallback = null
export default {
	created() {

		// performs lum_track initialization if needed
		if( ! window.lum_track ) {

			// pre-ininitalization
			window.lum_track=window.lum_track||function(){(lum_track.q=lum_track.q||[]).push([+new Date].concat([].slice.call(arguments)))}
			lum_track('config',{baseHref:lxpConfig.server})

			// post-initialization
			let elem = document.createElement("script")
			elem.crossOrigin = "use-credentials"
			elem.async = "async"
			elem.charset = "utf-8"
			elem.src = lxpConfig.server + "/lumis/portal/monitor/script/track.js?_=" + new Date().getTime()
			document.body.appendChild(elem)
		}

		// update lastPageViewTime
		lastPageviewTime = new Date().getTime()

		// bind on before unload to make sure we will collect the last leave page event
		window.addEventListener("beforeUnload", collectLeavePage)
	},
	beforeDestroy() {
		// avaid leaking the listener
		window.removeEventListener("beforeUnload", collectLeavePage)
	},
	watch: {

		// watch for route changes
		"$route" ( next, prev ){
			if ( prev ) {
				collectLeavePage ( window.location.origin + prev.path )
			}

			if ( next ) {
				collectPageView ( )
			}
		}
	}
}
</script>

<style>

</style>