<template>
	<header><style-display :styles="styles" /></header>
	<main>
		<textarea spellcheck="false" class="textArea" contenteditable="true" v-model="text" :style="styles"></textarea>
	</main>
	<footer>
		<div class="settings">
			<div class="column">
				<div>
					Font size
					<input v-model="fontSize" :min="fontSizeRange.min" :max="fontSizeRange.max" type="range" />
				</div>
				<div>Use VMin: <input v-model="ifResponsive" type="checkbox" name="ifResponsive" /></div>
				<div>Font color: <input v-model="color" type="color" /></div>
				<div>
					Do u want to use exact font? (if set, font family will be fall-back option) Only preinstalled fonts
					on ur device would work here.
					<input v-model="ifExactFontNeeded" placeholder="Arial" type="checkbox" name="fontName" />
				</div>
				<input v-if="ifExactFontNeeded" type="string" placeholder="Courier New" v-model="exactFontName" />
				<div>Font family: <option-selector :options="options.fontFamily" v-model="fontFamily" /></div>
				<div>Font style: <option-selector :options="options.fontStyle" v-model="fontStyle" /></div>
				<div>
					Apply font-variant: <input v-model="smallCaps" type="checkbox" name="smallCaps" />
					<template v-if="smallCaps">
						<div>Font Variant:</div>
						<option-selector :options="options.fontVariant" v-model="fontVariant" />
						<div>Font Variant Caps:</div>
						<option-selector :options="options.fontVariantCaps" v-model="fontVariantCaps" />
					</template>
				</div>
			</div>
			<div class="column">
				<div>
					Font weight:
					<option-selector :options="options.fontWeightInput" v-model="fontWeightInput" />
					<input
						v-if="fontWeightInput === 'number'"
						v-model="fontWeightNumber"
						:min="1"
						:max="9"
						type="range"
					/>
				</div>
				<div>
					Text decoration line:
					<option-selector :options="options.textDecorationLine" v-model="textDecorationLine" />
					<template v-if="textDecorationLine != 'none'">
						<div>Text decoration color:</div>
						<input v-model="textDecorationColor" type="color" />
						<div>Text decoration style:</div>
						<option-selector :options="options.textDecorationStyle" v-model="textDecorationStyle" />
					</template>
				</div>
				<div class="shadowCount">
					Shadows count: <input min="0" v-model="shadowCount" type="number" />
					<div>
						<text-shadow-config :useVmin="sizeMetric" @styleEmited="setShadow" :shadowCount="shadowCount" />
					</div>
				</div>
				<div>Text transform: <option-selector :options="options.textTransform" v-model="textTransform" /></div>
			</div>
		</div>
	</footer>
</template>

<script>
import OptionSelector from "@/components/OptionSelector.vue";
import StyleDisplay from "@/components/StyleDisplay.vue";
import TextShadowConfig from "@/components/TextShadowConfig";

export default {
	name: "app",
	components: {
		OptionSelector,
		StyleDisplay,
		TextShadowConfig
	},
	data() {
		return {
			options: {
				fontFamily: ["serif", "sans-serif", "monospace", "cursive", "fantasy"],
				fontStyle: ["normal", "italic", "oblique", "oblique 40deg"],
				fontVariant: ["normal", "italic", "oblique", "oblique 40deg"],
				fontVariantCaps: [
					"normal",
					"small-caps",
					"all-small-caps",
					"petite-caps",
					"all-petite-caps",
					"unicase",
					"titling-caps"
				],
				fontWeightInput: ["normal", "bold", "bolder", "lighter", "number", "initial", "inherit"],
				textDecorationLine: ["none", "underline", "overline", "line-through"],
				textDecorationStyle: ["initial", "solid", "double", "dotted", "dashed", "wavy"],
				textTransform: ["none", "capitalize", "uppercase", "lowercase"]
			},
			text: "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua",
			ifResponsive: false,
			color: "#ffffff",
			ifExactFontNeeded: false,
			exactFontName: "",
			fontFamily: "serif",
			fontSize: 72,
			fontStyle: "normal",
			smallCaps: false,
			fontVariant: "normal",
			fontVariantCaps: "normal",
			fontWeightInput: "normal",
			fontWeightNumber: 4,
			textDecorationLine: "none",
			textDecorationColor: "#000000",
			textDecorationStyle: "initial",
			textShadows: "unset",
			textTransform: "none",
			shadowCount: 0
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
		exactFontFamily() {
			let returnVal = this.fontFamily;
			if (this.exactFontName.length > 0) {
				returnVal = [this.exactFontName, this.fontFamily].join(", ");
			}
			return returnVal;
		},
		styles() {
			return {
				color: this.color,
				fontSize: this.fontSize + this.sizeMetric,
				fontFamily: this.exactFontFamily,
				fontStyle: this.fontStyle,
				fontVariant: this.fontVariant,
				fontVariantCaps: this.fontVariantCaps,
				fontWeight: this.fontWeight,
				textDecorationLine: this.textDecorationLine,
				textDecorationColor: this.textDecorationColor,
				textDecorationStyle: this.textDecorationStyle,
				textShadow: this.textShadows,
				textTransform: this.textTransform
			};
		}
	},
	watch: {
		ifResponsive: function () {
			this.fontSize = this.fontSizeRange.min;
		}
	},
	methods: {
		setShadow(val) {
			this.textShadows = val;
		}
	}
};
</script>

<style>
:root {
	--primaryFontColor: #f8f8f2;
	--textAreaBackgroundColor: #282a36;
	--bodyBackgroundColor: #44475a;
	--settingsColor: #6272a4;
}
body {
	margin: 0px 0px 0px 0px;
	color: var(--primaryFontColor);
	background-color: var(--bodyBackgroundColor);
}
main {
	width: 90%;
	min-height: 90%;
	color: var(--primaryFontColor);
	border-radius: 1vmin;
	left: 0;
	right: 0;
	margin-left: auto;
	margin-right: auto;
	margin-top: 2.5%;
	margin-bottom: 2.5%;
}
main > * {
	padding: 1vmin 1vmin 1vmin 1vmin;
}
textArea {
	width: 100%;
	height: 80vh;
	padding: 0px 0px 0px 0px;
	margin: 0px 0px 0px 0px;
	border-radius: 1vmin;
	border: 0px;
	background-color: var(--textAreaBackgroundColor);
	text-align: center;
}
.settings {
	display: flex;
	width: 100%;
	background-color: var(--settingsColor);
}
.column {
	width: 50%;
	margin-left: 2rem;
	margin-right: 2rem;
	text-align: right;
}
.column + .column {
	text-align: left;
}
.shadowCount {
	max-height: 10vmin;
	overflow: auto;
	background-color: rgba(0, 0, 0, 0.2);
}
footer {
	bottom: 0;
	position: absolute;
	margin: 0 auto;
	margin-left: 10rem;
	margin-right: 10rem;
	border-radius: 1vmin;
	padding: 1vmin;
}
</style>
