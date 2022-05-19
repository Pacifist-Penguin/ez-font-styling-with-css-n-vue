<template>
	<button class="btn"><slot /></button>
</template>

<style scoped>
.btn {
	position: relative;

	display: block;
	margin: 30px auto;
	padding: 0;

	overflow: hidden;

	border-width: 0;
	outline: none;
	border-radius: 2px;
	box-shadow: 0 1px 4px rgba(0, 0, 0, 0.6);

	background-color: #2ecc71;
	color: #ecf0f1;

	transition: background-color 0.3s;
}

.btn:hover,
.btn:focus {
	background-color: #27ae60;
}

.btn > * {
	position: relative;
}

.btn span {
	display: block;
	padding: 12px 24px;
}

.btn:active:before {
	width: 120%;
	padding-top: 120%;
	transition: width 0.2s ease-out, padding-top 0.2s ease-out;
}
</style>
