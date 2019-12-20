<template>
	<item-details :id="id" />
</template>

<script>
import ItemDetails from "~/components/ItemDetails.vue"
export default {
	components: {
		ItemDetails
	},
	data() {
		return {
			id: this.$route.params.id
		}
	}
}
</script>

<style>

</style>