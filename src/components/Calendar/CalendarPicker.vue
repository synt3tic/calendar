<script setup lang="ts">
import { IonDatetime } from '@ionic/vue'

interface Props {
	date: string
	format: 'date' | 'time'
}
interface Emit {
	(emit: 'cancel'): void
	(emit: 'select', date: string): void
}

defineProps<Props>()
const emit = defineEmits<Emit>()

const selectDate = (event: CustomEvent) => {
	emit('select', event.detail.value)
}
</script>

<template>
	<div
		class="backdrop"
		@click.self="emit('cancel')"
	>
		<ion-datetime
			:value="date"
			:presentation="format"
			show-default-buttons
			done-text="Готово"
			cancel-text="Отмена"
			@ion-cancel="emit('cancel')"
			@ion-change="selectDate"
		/>
	</div>
</template>

<style scoped lang="scss">
.backdrop {
	position: fixed;
	top: 0;
	right: 0;
	bottom: 0;
	left: 0;
	background-color: rgba(0, 0, 0, 0.3);
	backdrop-filter: blur(2px);
	padding: 24px;

	display: flex;
	align-items: center;
	justify-content: center;

	* {
		border-radius: 16px;
		min-width: 100%;

		animation: slide-in 0.15s ease-in-out;
	}
}

@keyframes slide-in {
	from {
		transform: translateY(100%);
	}
	to {
		transform: translateY(0);
	}
}
</style>
