<template>
	<div v-if="smoothie" class="edit-smoothie container z-depth-1">
		<h2 class="indigo-text center-align">Edit {{ smoothie.title }} Smoothie</h2>
		<AppForm :propTitle=smoothie.title
				 :propSlug=smoothie.slug
				 :propIngredient= smoothie.ingredients
				 :propType=type
				 @submit="editSmoothie($event)"/>
	</div>
</template>

<script>
	import AppForm from '@/components/AppSmoothieForm'
	import db from '@/firebase/init'

	export default {
		name: 'EditSmoothie',
		components: {
		 AppForm
		},
		data() {
			return {
				smoothie: null,
				type: 'edit'
			}
		},
		methods: {
			editSmoothie(e) {
				db.collection('smoothies').doc(this.smoothie.id).update({
					title: e.title,
					slug: e.slug,
					ingredients: e.ingredients
				}).then(() => {
					this.$router.push({name: 'Index'})
				}).catch(err => {
					console.log(err)
				})
			}
		},
		created() {
			let ref = db.collection('smoothies').where('slug', '==', this.$route.params.smoothie_slug)
			ref.get().then(snapshot => {
				snapshot.forEach(doc => {
					this.smoothie = doc.data()
					this.smoothie.id = doc.id
				})
			})
		}
	}
</script>

<style>
	.edit-smoothie {
		margin-top: 60px;
		padding: 20px;
		max-width: 500px;
	}

	.edit-smoothie h2 {
		font-size: 2em;
		margin: 20px auto;
	}

</style>