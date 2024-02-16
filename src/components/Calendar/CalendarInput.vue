<script setup lang="ts">
import { computed } from 'vue'

interface Props {
	date: string
	isTime?: boolean
	timesLength?: number
}
interface Emit {
	(emit: 'remove'): void
}

const props = defineProps<Props>()
const emit = defineEmits<Emit>()

const dateFormatOptions: Intl.DateTimeFormatOptions = {
	day: '2-digit',
	year: '2-digit',
	month: '2-digit'
}

const timeFormatOptions: Intl.DateTimeFormatOptions = {
	hour: '2-digit',
	minute: '2-digit'
}

const formattedDate = computed(() => {
	const options = props.isTime ? timeFormatOptions : dateFormatOptions
	const method = props.isTime ? 'toLocaleTimeString' : 'toLocaleDateString'

	return new Date(props.date)[method]('ru', options)
})
</script>

<template>
	<div class="date-input">
		{{ formattedDate }}
		<button
			v-if="isTime && timesLength && timesLength > 1"
			@click.stop="emit('remove')"
		>
			<svg
				viewBox="0 0 12 12"
				width="12"
				height="12"
				fill="none"
				xmlns="http://www.w3.org/2000/svg"
			>
				<path
					d="M3 3L6 6M6 6L9 9M6 6L9 3M6 6L3 9"
					stroke="var(--svg-col, #646cff)"
					stroke-width="1.7"
					stroke-linecap="round"
					stroke-linejoin="round"
				/>
			</svg>
		</button>
	</div>
</template>

<style scoped lang="scss">
.date-input {
	display: flex;
	justify-content: space-between;
	align-items: center;
	height: fit-content;
	cursor: pointer;
	background-color: rgba(215, 197, 250, 0.274);

	border-radius: 8px;
	padding: 8px;
	border: 1px solid rgba(86, 69, 119, 0.274);

	* {
		margin: 0 !important;
	}
}
</style>
