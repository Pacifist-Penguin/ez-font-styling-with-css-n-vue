<template>
	<div v-for="(shadow, index) in shadowCount" :key="index">
		<input v-model="arrayOfShadows[index].XOffset" type="number" placeholder="x-offset" />
		<input v-model="arrayOfShadows[index].YOffset" type="number" placeholder="y-offset" />
		<input v-model="arrayOfShadows[index].blurRadius" type="number" placeholder="blur radius" />
		<input v-model="arrayOfShadows[index].color" type="color" placeholder="color" />
	</div>
</template>

<script>
export default {
	name: "TextShadowConfig",
	emits: {
		shadowStyle: (shadowStyle) => typeof shadowStyle === "object"
	},
	props: {
		shadowCount: {
			type: Number,
			required: false,
			default: 0
		}
	},
	computed: {},
	data() {
		return {
			arrayOfShadows: []
		};
	},
	methods: {
		generateArrayOfShadows() {
			const arr = [];
			for (let index = 0; index < this.shadowCount; index++) {
				arr.push({});
			}
			this.arrayOfShadows = arr;
		}
	},
	watch: {
		shadowCount: function () {
			this.generateArrayOfShadows();
		}
	}
};
</script>
