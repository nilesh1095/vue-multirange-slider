<template>
	<div class="multi-range-slider">
		<div class="slidecontainer">
			<input
				type="range"
				v-on:change="leftInputChangeHandler"
				v-on:input="leftInputChangeHandler"
				v-model="leftSliderValue"
				:class="[`${name}-left-slider`]"
				min="0"
				:max="maxSliderValue"
				:step="steps"
				class="left-slider"
				v-if="!singleRangeSlider"
			/>
		</div>
		<div class="slidecontainer">
			<input
				type="range"
				v-on:change="rightInputChangeHandler"
				v-on:input="rightInputChangeHandler"
				v-model="rightSliderValue"
				:class="[`${name}-right-slider`]"
				min="0"
				:max="maxSliderValue"
				:step="steps"
				class="right-slider"
			/>
		</div>
		<div class="slider-illusion-container">
			<div class="track"></div>
			<div class="range" :style="{ left: `${setLeftThumb}%`, right: `${setRightThumb}%` }"></div>
			<div class="thumb-left " :style="{ left: `${setLeftThumb}%` }" v-if="!singleRangeSlider"></div>
			<div class="thumb-right " :style="{ right: `${setRightThumb}%` }"></div>
		</div>
	</div>
</template>

<script>
export default {
	data() {
		return {
			leftSliderValue: 0,
			rightSliderValue: 0
		};
	},
	computed: {
		setSliderWidth() {
			return (this.leftSliderValue / this.maxSliderValue) * 100;
		},
		setLeftThumb() {
			return (this.leftSliderValue / this.maxSliderValue) * 100;
		},
		setRightThumb() {
			return (this.rightSliderValue / this.maxSliderValue) * 100;
		}
	},
	watch: {
		reset() {
			this.leftSliderValue = 0;
			this.rightSliderValue = 0;
		}
	},
	props: {
		maxSliderValue: {
			type: Number,
			required: true,
			default: 0
		},
		name: {
			type: String,
			required: true,
			default: ''
		},
		initialValue: {
			type: Object,
			required: false,
			default: null
		},
		margin: {
			//This prop will configure the minimum margin between high and low price
			type: Number,
			required: true,
			default: 150000
		},
		reset: {
			type: Boolean,
			required: false,
			default: false
		},
		steps: {
			type: Number,
			required: true,
			default: 1000
		},
		singleRangeSlider: {
			type: Boolean,
			required: false,
			default: false
		}
	},
	mounted() {
		this.$nextTick(() => {
			if (!this.singleRangeSlider) {
				this.leftSliderValue = this.initialValue ? this.initialValue.leftSliderValue : 0;
				document.querySelector(`.${this.name}-left-slider`).addEventListener('change', this.inputChangeHandler);
			}
			this.rightSliderValue = this.initialValue ? this.maxSliderValue - this.initialValue.rightSliderValue : 0;
			document.querySelector(`.${this.name}-right-slider`).addEventListener('change', this.inputChangeHandler);
		});
	},
	methods: {
		leftInputChangeHandler(e) {
			if (this.leftSliderValue >= this.maxSliderValue - this.rightSliderValue - this.margin) {
				this.leftSliderValue = this.maxSliderValue - this.rightSliderValue - this.margin;
				e.preventDefault();
				return false;
			}
		},
		rightInputChangeHandler(e) {
			if (this.rightSliderValue >= this.maxSliderValue - this.leftSliderValue - this.margin) {
				this.rightSliderValue = this.maxSliderValue - this.leftSliderValue - this.margin;
				e.preventDefault();
				return false;
			}
		},
		inputChangeHandler() {
			const data = {
				leftSliderValue: this.leftSliderValue,
				rightSliderValue: `${this.maxSliderValue - this.rightSliderValue}`
			};
			this.$emit('sliderValueChanged', data);
		}
	}
};
</script>

<style>
.multi-range-slider {
	position: relative;
}
.multi-range-slider .slidecontainer {
	/* position: relative; */
	width: 100%;
}

.multi-range-slider .left-slider,
.multi-range-slider .right-slider {
	position: relative;
	width: 100%;
}

.multi-range-slider .right-slider {
	transform: rotate(180deg);
}
.multi-range-slider .slider-illusion-container {
	position: relative;
	margin: 0 10px;
	/* margin-top: 18px;
	margin-bottom: 12px; */
}
.multi-range-slider .slider-illusion-container .track {
	width: 100%;
	border-radius: 12px;
	height: 4px;
	background: #eeeeee;
}
.multi-range-slider .slider-illusion-container .range {
	border-radius: 12px;
	height: 4px;
	position: absolute;
	top: 0;
	background: #395bb6;
}

.multi-range-slider .slider-illusion-container .thumb-left,
.multi-range-slider .slider-illusion-container .thumb-right {
	position: absolute;
	border-radius: 100%;
}

.multi-range-slider .thumb-left,
.multi-range-slider .thumb-right {
	background: #395bb6;
	border: 3px solid #ffffff;
	box-sizing: border-box;
	box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.12);
	width: 24px;
	height: 24px;
	top: -10px;
}

.multi-range-slider .thumb-left {
	transform: translateX(-12px);
}

.multi-range-slider .thumb-right {
	transform: translateX(12px);
}

.multi-range-slider input[type='range'] {
	pointer-events: none;
	position: absolute;
	z-index: 2;
	width: 100%;
	top: -8px;
	opacity: 0;
}

.multi-range-slider input[type='range']::-webkit-slider-thumb {
	pointer-events: all;
	border-radius: 0;
	border: 0 none;
	-webkit-appearance: none;
	cursor: pointer;
	z-index: 3;
}
.multi-range-slider input[type='range']::-moz-range-thumb {
	pointer-events: all;
	border-radius: 0;
	border: 0 none;
	-webkit-appearance: none;
	cursor: pointer;
	z-index: 3;
}
</style>
