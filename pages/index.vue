<template>
	<div>
		<ul>
			<li v-for="item in data" v-bind:key="item.id">
				<item-preview :id="item.id" />
			</li>
		</ul>
	</div>
</template>

<script>
import { data } from "../sample.data"
import ItemPreview from "~/components/ItemPreview.vue"

export default {
  components: {
	  ItemPreview
  },
  data() {
	  return { data }
  }
}
</script>

<style>
</style>
