<template>
	<div>
		<nuxt-link :to="`/items/${ item.id }`">
			<img :src="item.image" />
			<div>
				{{ item.title }}
			</div>
		</nuxt-link>
	</div>
</template>

<script>
import { data } from "../sample.data"
export default {
	props: {
		id: {
			type: [ String, Number ],
			required: true
		}
	},
	data() {
		let item = data.filter(d => d.id == this.id)[0]
		return { item }
	}
}
</script>

<style>

</style>