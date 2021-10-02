<template>
	<img alt="Vue logo" src="./assets/logo.png" />
	<button @click="resetHandler">Reset</button>
	<MultiRangeSlider
		:singleRangeSlider="false"
		@sliderChangeEvent="sliderValues"
		@sliderInputEvent="sliderValues"
		:reset="resetRangeFilter"
		:margin="1"
		:steps="1"
		:initialValue="sliderInitialValues"
		name="price"
		:maxSliderValue="100"
	/>
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
