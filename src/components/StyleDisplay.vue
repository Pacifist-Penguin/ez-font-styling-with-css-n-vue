<template>
	<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.2.0/styles/default.min.css" />
	<div ref="highlightedStyleContainer" class="copyStyles"></div>
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
		expirementalStyles: function () {
			this.$refs.highlightedStyleContainer.innerHTML = this.expirementalStyles;
		},
		styles: function () {
			this.expirementalStyles = this.convert(this.styles);
		}
	},
	mounted() {
		this.convert(this.styles);
	}
};
</script>
<style scoped>
.copyStyles {
	width: max(25%, 16rem);
	border-radius: 2vmin;
	background-color: var(--settingsColor);
	white-space: pre-wrap;
	padding-left: 3vmin;
	padding-bottom: 1rem;
	right: 0;
	bottom: 25vmin;
	z-index: 2;
	position: absolute;
}
</style>
