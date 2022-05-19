<template>
	<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.2.0/styles/default.min.css" />
	<div v-html="expirementalStyles" ref="highlightedStyleContainer" class="copyStyles"></div>
</template>

<script>
import hljs from "highlight.js/lib/core";
import css from "highlight.js/lib/languages/css";
import postcss from "postcss";
import postcssJs from "postcss-js";

hljs.registerLanguage("css", css);

export default {
	name: "StyleDisplay",
	props: {
		styles: {
			type: Object,
			required: true
		}
	},
	data() {
		return {
			expirementalStyles: ""
		};
	},
	// couldn't make postcss to work with computed property
	methods: {
		convert(style) {
			postcss()
				.process(style, { parser: postcssJs })
				.then((result) => {
					this.expirementalStyles = hljs.highlight(result.css, { language: "css" }).value;
				});
		}
	},
	watch: {
		styles: function () {
			this.convert(this.styles);
		}
	},
	mounted() {
		this.convert(this.styles);
	}
};
</script>

<style scoped>
.copyStyles {
	width: min(90%, 24rem);
	height: 75%;
	border-radius: 2vmin;
	background-color: var(--settingsColor);
	white-space: pre-wrap;
	padding: 2rem;
	margin: auto;
	display: block;
}
</style>
