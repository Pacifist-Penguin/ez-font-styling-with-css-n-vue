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
	},
	data() {
		return {
			selectedOption: "1"
		};
	},
	props: {
		options: {
			type: Array,
			required: true
		}
	},
	emits: {
		selectedOption: () => typeof selectedOption === "string"
	},
	watch: {
		selectedOption: function (value) {
			this.$emit(`update:selected`, value);
		}
	}
};
</script>
