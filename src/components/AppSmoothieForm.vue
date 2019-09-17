<template>
	<div class="wrap-smoothie">
		<form @submit.prevent="isSmoothie">
			<div class="field title">
				<label for="title">Smoothie title:</label>
				<input type="text" name="title" v-model="title">
			</div>
			<div v-for="(ing, index) in ingredients" class="field ingredient" :key="index">
				<label for="ingredient">Ingredient:</label>
				<input type="text" name="ingredient" v-model="ingredients[index]">
				<i class="material-icons delete" @click="deleteIng(ing)">delete</i>
			</div>
			<div class="field add-ingredient">
				<label for="add-ingredient">Add an ingredient (press enter to add):</label>
				<input type="text" name="add-ingredient" @keydown.enter.prevent="addIng" v-model="another">
			</div>
			<div class="field center-align">
				<p v-if="feedback" class="red-text">{{ feedback }}</p>
				<button class="btn pink">{{propType}} Smoothie</button>
			</div>
		</form>
	</div>

</template>

<script>
	import slugify from 'slugify'

	export default {
		name: "AppSmoothieForm",
		props: {
			propTitle: {
				default: null
			},
			propSlug: {
				default: null
			},
			propIngredient: {
				default: null
			},
			propType: {
				type: String
			}
		},
		data() {
			return {
				title: this.propTitle,
				ingredients: this.propIngredient,
				slug: this.propSlug,
				another: null,
				feedback: null
			}
		},
		methods: {
			isSmoothie() {
				if(this.title) {
					this.feedback = null

					this.slug = slugify(this.title, {
						replacement: '-',
						remove: /[$*_+~.()'"!\-:@]/g,
						lower: true
					});

					this.$emit('submit', {
						title: this.title,
						slug: this.slug,
						ingredients: this.ingredients
					});

				} else {
					this.feedback = 'You must enter a smoothie title'
				}
			},
			addIng() {
				if(this.another) {
					this.ingredients.push(this.another)
					this.another = null
					this.feedback = null
				} else {
					this.feedback = 'You must enter a value to add another ingredient'
				}
			},
			deleteIng(ing) {
				this.ingredients = this.ingredients.filter(ingredient => {
					return ingredient != ing
				})
			}
		}
	}
</script>

<style>
	.wrap-smoothie .field {
		margin: 20px auto;
		position: relative;
	}

	.wrap-smoothie .delete {
		position: absolute;
		right: 0;
		bottom: 16px;
		color: #aaa;
		font-size: 1.4em;
		cursor: pointer;
	}
</style>