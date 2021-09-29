<template>
	<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.2.0/styles/default.min.css" />
	<div ref="highlightedStyleContainer" class="copyStyles"></div>
</template>

<script>
import hljs from "highlight.js/lib/core";
import css from "highlight.js/lib/languages/css";
hljs.registerLanguage("css", css);

export default {
	name: "StyleDisplay",
	props: {
		styles: {
			type: Object,
			required: true
		}
	},
	computed: {
		stringifiedStyles() {
			return JSON.stringify(this.styles);
		},
		removedQuotes() {
			return this.stringifiedStyles.replace(/['"]+/g, "");
		},
		replacedComasWithSemiColumns() {
			return this.removedQuotes.replace(/[',]+/g, "; \n");
		},
		convertedToKebabCase() {
			return this.replacedComasWithSemiColumns
				.replace(/((?<=[a-z\d])[A-Z]|(?<=[A-Z\d])[A-Z](?=[a-z]))/g, "-$1")
				.toLowerCase();
		},
		highlightedStyle() {
			return hljs.highlight(this.convertedToKebabCase, { language: "css" }).value;
		}
	},
	watch: {
		highlightedStyle: function () {
			this.$refs.highlightedStyleContainer.innerHTML = this.highlightedStyle;
		}
	},
	mounted() {
		this.$refs.highlightedStyleContainer.innerHTML = this.highlightedStyle;
	}
};
</script>
<style>
.copyStyles {
	width: 50%;
	background-color: #0664b1;
	white-space: pre-wrap;
}
.copyStyles > .hljs-attribute {
	margin-left: 3vmin;
}
</style>
