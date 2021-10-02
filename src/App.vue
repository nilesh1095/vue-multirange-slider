<template>
	<img alt="Vue logo" src="./assets/logo.png" />
	<button @click="resetHandler">Reset</button>
	<MultiRangeSlider @sliderChangeEvent="sliderValues" @sliderInputEvent="sliderValues" name="price" />
</template>

<script>
import MultiRangeSlider from './components/MultiRangeSlider.vue';

export default {
	name: 'App',
	data() {
		return {
			resetRangeFilter: false,
			sliderInitialValues: null
		};
	},
	components: {
		MultiRangeSlider
	},
	mounted() {},
	methods: {
		sliderValues(data) {
			console.log(data);
		},
		resetHandler() {
			this.resetRangeFilter = true;
			setTimeout(() => {
				this.resetRangeFilter = false;
			});
		}
	}
};
</script>

<style></style>
