<template>
	<div class="parentElement" v-for="(shadow, index) in shadowCount" :key="index">
		<input class="inputNumber" v-model="arrayOfShadows[index].XOffset" type="number" placeholder="x-offset" />
		<input class="inputNumber" v-model="arrayOfShadows[index].YOffset" type="number" placeholder="y-offset" />
		<input class="inputNumber" v-model="arrayOfShadows[index].blurRadius" type="number" placeholder="blur radius" />
		<input v-model="arrayOfShadows[index].color" type="color" placeholder="color" />
	</div>
</template>

<script>
export default {
	name: "TextShadowConfig",
	emits: {
		styleEmited: (styles) => typeof styles === "string"
	},
	props: {
		shadowCount: {
			type: Number,
			required: true,
			default: 1
		},
		useVmin: {
			type: String,
			required: true,
			default: "px"
		}
	},
	computed: {
		style() {
			const arr = [];
			this.arrayOfShadows.forEach((element, index) => {
				for (const key in element) {
					if (Object.hasOwnProperty.call(element, key)) {
						let string = element[key];
						if (key === "XOffset" || key === "YOffset" || key === "blurRadius") {
							string += this.useVmin;
						}
						if (key === "color" && this.shadowCount - 1 > index) {
							string += ",";
						}
						arr.push(string);
					}
				}
			});
			const string = arr.join(" ");
			return string;
		}
	},
	data() {
		return {
			arrayOfShadows: []
		};
	},
	watch: {
		shadowCount: function (state, prevState) {
			const diff = state - prevState;

			if (state > prevState) {
				for (let i = 0; i < diff; i++) {
					this.arrayOfShadows.push({
						XOffset: 0,
						YOffset: 0,
						blurRadius: 0,
						color: "#ffffff"
					});
				}
			} else {
				for (let i = 0; i > diff; i--) {
					this.arrayOfShadows.pop();
				}
			}
		},
		style: function () {
			//emit this if there's no shadows
			if (this.style === "") {
				this.$emit("styleEmited", "unset");
			}
			//else emit styles
			else {
				this.$emit("styleEmited", this.style);
			}
		}
	}
};
</script>

<style scoped>
.inputNumber {
	width: 5vmin;
}
.parentElement {
	justify-content: center;
	display: flex;
	background-color: rgba(0, 0, 0, 0.2);
}
</style>
