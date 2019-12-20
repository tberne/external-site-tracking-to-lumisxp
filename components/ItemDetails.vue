<template>
	<div>
		<img :src="item.image" />
		<div>
			{{ item.title }}
		</div>
		<div>
			{{ item.price }}
		</div>
		<ul>
			<li v-for="cat in item.categories" v-bind:key="cat">
				{{ cat }}
			</li>
		</ul>
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