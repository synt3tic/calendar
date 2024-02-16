<script setup lang="ts"></script>

<template>
	<button class="button"><slot /></button>
</template>

<style scoped lang="scss">
.button {
	border-radius: 8px;
	border: 1px solid transparent;
	padding: 0.6em 1.2em;
	font-size: 0.85rem;
	background-color: #646cff;
	color: white;

	transition: all 0.15s ease-in-out;
	font-family: 'Montserrat', sans-serif;

	&:active {
		background-color: #535bf2;
		transform: scale(0.98);
	}

	@media (hover: hover) {
		&:hover {
			border-color: #646cff;
		}
	}
}
</style>
