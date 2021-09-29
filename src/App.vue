<template>
	<header>This tool allows u to easily configure ur fonts.</header>
	<main>
		<div style="display: flex">
			<textarea class="textArea" contenteditable="true" v-model="text" :style="styles"></textarea>
			<style-display :styles="styles" />
		</div>
		<div>
			<div>Font size</div>
			<input v-model="fontSize" :min="fontSizeRange.min" :max="fontSizeRange.max" type="range" />
			<div>Use VMin: <input v-model="ifResponsive" type="checkbox" name="ifResponsive" /></div>
			<div>Font family:</div>
			<select v-model="fontFamily">
				<option value="serif">Serif</option>
				<option value="sans-serif">Sans-serif</option>
				<option value="monospace">Monospace</option>
				<option value="cursive">Cursive</option>
				<option value="fantasy">Fantasy</option>
			</select>
			<div>Font style:</div>
			<select v-model="fontStyle">
				<option value="normal">normal</option>
				<option value="italic">italic</option>
				<option value="oblique">oblique</option>
				<option value="oblique 40deg">oblique 40 degree</option>
			</select>
			<div>Apply font-variant: <input v-model="smallCaps" type="checkbox" name="smallCaps" /></div>
			<template v-if="smallCaps">
				<div>Font Variant:</div>
				<select v-model="fontVariant">
					<option value="normal">normal</option>
					<option value="italic">italic</option>
					<option value="oblique">oblique</option>
					<option value="oblique 40deg">oblique 40 degree</option>
				</select>
				<div>Font Variant Caps:</div>
				<select v-model="fontVariantCaps">
					<option value="normal">normal</option>
					<option value="small-caps">small-caps</option>
					<option value="all-small-caps">all-small-caps</option>
					<option value="petite-caps">petite-caps</option>
					<option value="all-petite-caps">all-petite-caps</option>
					<option value="unicase">unicase</option>
					<option value="titling-caps">titling-caps</option>
				</select>
			</template>
			<div>Font weight:</div>
			<select v-model="fontWeightInput">
				<option value="normal">normal</option>
				<option value="bold">bold</option>
				<option value="bolder">bolder</option>
				<option value="lighter">lighter</option>
				<option value="number">number</option>
				<option value="initial">initial</option>
				<option value="inherit">inherit</option>
			</select>
			<input v-if="fontWeightInput === 'number'" v-model="fontWeightNumber" :min="1" :max="9" type="range" />
			<div>Text decoration line:</div>
			<select v-model="textDecorationLine">
				<option value="none">none</option>
				<option value="underline">underline</option>
				<option value="overline">overline</option>
				<option value="line-through">line-through</option>
			</select>
			<template v-if="textDecorationLine != 'none'">
				<div>Text decoration color:</div>
				<input v-model="textDecorationColor" type="color" />
				<div>Text decoration style:</div>
				<select v-model="textDecorationStyle">
					<option value="initial">initial</option>
					<option value="solid">solid</option>
					<option value="double">double</option>
					<option value="dotted">dotted</option>
					<option value="dashed">dashed</option>
					<option value="wavy">wavy</option>
				</select>
			</template>
			<!-- PLACE FOR TEXT SHADOW -->
			<select v-model="textTransform">
				<option value="none">none</option>
				<option value="capitalize">capitalize</option>
				<option value="uppercase">uppercase</option>
				<option value="lowercase">lowercase</option>
			</select>
			<!-- test of component -->
			<div>Test:</div>
			<option-selector v-model:selected="test" :options="['1', '2', '3', '4']" />
		</div>
		<div></div>
	</main>
</template>

<script>
import OptionSelector from "@/components/OptionSelector.vue";
import StyleDisplay from "@/components/StyleDisplay.vue";

export default {
	name: "app",
	components: {
		OptionSelector,
		StyleDisplay
	},
	data() {
		return {
			text: "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua",
			ifResponsive: false,
			fontFamily: "serif",
			fontSize: 16,
			fontStyle: "normal",
			smallCaps: false,
			fontVariant: "normal",
			fontVariantCaps: "normal",
			fontWeightInput: "normal",
			fontWeightNumber: 4,
			textDecorationLine: "none",
			textDecorationColor: "#000000",
			textDecorationStyle: "initial",
			textTransform: "none",
			test: "1"
		};
	},
	computed: {
		fontSizeRange() {
			return this.ifResponsive
				? {
						min: 2,
						max: 36
				  }
				: {
						min: 8,
						max: 72
				  };
		},
		sizeMetric() {
			return this.ifResponsive ? "vmin" : "px";
		},
		fontWeight() {
			return this.fontWeightInput === "number" ? this.fontWeightNumber * 100 : this.fontWeightInput;
		},
		styles() {
			return {
				fontSize: this.fontSize + this.sizeMetric,
				fontFamily: this.fontFamily,
				fontStyle: this.fontStyle,
				fontVariant: this.fontVariant,
				fontVariantCaps: this.fontVariantCaps,
				fontWeight: this.fontWeight,
				textDecorationLine: this.textDecorationLine,
				textDecorationColor: this.textDecorationColor,
				textDecorationStyle: this.textDecorationStyle
			};
		}
	},
	watch: {
		ifResponsive: function () {
			this.fontSize = this.fontSizeRange.min;
		}
	}
};
</script>

<style>
body {
	margin: 0px 0px 0px 0px;
	background-color: #31393c;
}
main {
	width: 90%;
	height: 90%;
	background-color: #2176ff;
	margin: auto;
	border-radius: 1vmin;
}
main > * {
	padding: 1vmin 1vmin 1vmin 1vmin;
}
textArea {
	width: 50%;
	height: 40vh;
	padding: 0px 0px 0px 0px;
	margin: 0px 0px 0px 0px;
	border: 0px;
	background-color: #33a1fd;
}
</style>
