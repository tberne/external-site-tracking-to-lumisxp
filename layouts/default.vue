<template>
	<div>
		<nav>
			<ul>
				<li>
					<nuxt-link to="/">Home</nuxt-link>
				</li>
			</ul>
		</nav>
		<!--<lum-track-page-views />-->
		<nuxt />
	</div>
</template>

<script>
import { lxpConfig } from "../lxp.config"
// import LumTrackPageViews from "../components/LumTrackPageViews.vue"
let onUnloadCallback = null
export default {
	components: {
	// 	LumTrackPageViews
	},
	created() {
		if( ! window.lum_track ) {
			window.lum_track=window.lum_track||function(){(lum_track.q=lum_track.q||[]).push([+new Date].concat([].slice.call(arguments)))};
			lum_track('config',{baseHref:lxpConfig.server});

			let elem = document.createElement("script")
			elem.src = lxpConfig.server + "/lumis/portal/monitor/script/track.js?_=" + new Date().getTime()
			document.body.appendChild(elem)
		}

		this.lastPageviewTime = new Date().getTime()
		onUnloadCallback = () => {
			let duration = new Date().getTime() - this.lastPageviewTime
			lum_track("event", "lumis.portal.monitor.ev.leavepage", {
				"lum_event.duration": duration,
				"lum_request.mode.id": "0"
			})
		}
		window.onbeforeunload = onUnloadCallback
	},
	data() {
		return {
			lastPageviewTime: new Date().getTime()
		}
	},
	beforeDestroy() {
		delete window.onbeforeunload
		onUnloadCallback = null
	},
	watch: {
		"$route" ( next, prev ){
			if ( prev ) {
				let duration = new Date().getTime() - this.lastPageviewTime
				lum_track("event", "lumis.portal.monitor.ev.leavepage", {
					"lum_event.duration": duration,
					"lum_client.url": window.location.origin + prev.path,
					"lum_request.mode.id": "0"
				})
			}

			if ( next ) {
				lum_track("event", "lumis.portal.monitor.ev.pageview", {
					"lum_request.mode.id": "0"
				})
			}
		}
	}
}
</script>

<style>
html {
  font-family: 'Source Sans Pro', -apple-system, BlinkMacSystemFont, 'Segoe UI',
    Roboto, 'Helvetica Neue', Arial, sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}

*,
*:before,
*:after {
  box-sizing: border-box;
  margin: 0;
}

.button--green {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #3b8070;
  color: #3b8070;
  text-decoration: none;
  padding: 10px 30px;
}

.button--green:hover {
  color: #fff;
  background-color: #3b8070;
}

.button--grey {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #35495e;
  color: #35495e;
  text-decoration: none;
  padding: 10px 30px;
  margin-left: 15px;
}

.button--grey:hover {
  color: #fff;
  background-color: #35495e;
}
</style>
