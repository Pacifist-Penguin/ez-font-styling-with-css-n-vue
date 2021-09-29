<template>
	<select v-model="selectedOption">
		<option v-for="option in options" :key="option" :value="option">
			{{ option }}
		</option>
	</select>
</template>

<script>
export default {
	name: "OptionSelector",
	created() {
		this.$emit(`update:selected`, this.options[0]);
		this.selectedOption = this.options[0];
	},
	data() {
		return {
			selectedOption: null
		};
	},
	props: {
		options: {
			type: Array,
			required: true
		}
	},
	emits: {
		"update:selected": (selectedOption) => typeof selectedOption === "string"
	},
	watch: {
		selectedOption: function (value) {
			this.$emit(`update:selected`, value);
		}
	}
};
</script>
