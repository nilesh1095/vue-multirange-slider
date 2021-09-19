<template>
	<img alt="Vue logo" src="./assets/logo.png" />
	<button @click="resetRangeFilter = true">Reset</button>
	<MultiRangeSlider
		:singleRangeSlider="false"
		@sliderValueChanged="sliderValues"
		:reset="resetRangeFilter"
		:margin="6000"
		:steps="1000"
		:initialValue="sliderInitialValues"
		name="price"
		:maxSliderValue="100000"
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
		}
	}
};
</script>

<style></style>
