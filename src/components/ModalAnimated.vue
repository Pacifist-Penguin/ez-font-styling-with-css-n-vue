<template>
	<div class="modalBackdrop">
		<slot />
	</div>
</template>

<script>
export default {
	name: "ModalAnimated"
};
</script>

<style scoped>
.modalBackdrop {
	position: fixed;
	top: 0;
	bottom: 0;
	left: 0;
	right: 0;
	margin: 0;
	height: 100vh;
	width: 100vw;
	z-index: 200;
	box-sizing: border-box;
	background-color: rgba(0, 0, 0, 0);
	animation: slide-in 0.4s linear 1;
	background-color: rgba(0, 0, 0, 0.6);
	display: flex;
}

@keyframes slide-in {
	from {
		background-color: rgba(0, 0, 0, 0);
	}
	to {
		background-color: rgba(0, 0, 0, 0.6);
	}
}
</style>
