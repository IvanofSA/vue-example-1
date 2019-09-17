<template>
	<div class="add-smoothie container z-depth-1">
		<h2 class="center-align indigo-text">Add New Smoothie Recipe</h2>

		<AppForm :propTitle=title
				 :propSlug=slug
				 :propIngredient=ingredients
				 :propType=type
				 @submit="addSmoothie($event)"/>

	</div>
</template>

<script>
	import AppForm from '@/components/AppSmoothieForm'
	import db from '@/firebase/init'

	export default {
		name: 'AddSmoothie',
		components: {
			AppForm
		},
		data() {
			return {
				title: null,
				ingredients: [],
				slug: null,
				type: 'add'
			}
		},
		methods: {
			addSmoothie(e) {
				console.log(e);

				db.collection('smoothies').add({
					title: e.title,
					ingredients: e.ingredients,
					slug: e.slug
				}).then(() => {
					this.$router.push({name: 'Index'})
				}).catch(err => {
					console.log(err)
				})
			}
		}
	}
</script>

<style>
	.add-smoothie {
		margin-top: 60px;
		padding: 20px;
		max-width: 500px;
	}

	.add-smoothie h2 {
		font-size: 2em;
		margin: 20px auto;
	}
</style>