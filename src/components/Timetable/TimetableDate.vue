<script setup lang="ts">
import { DateItem } from '@/types/timetableTypes'
import CalendarInput from '../Calendar/CalendarInput.vue'
import CalendarPicker from '../Calendar/CalendarPicker.vue'
import UiButton from '../UI/UiButton.vue'
import { ref } from 'vue'

interface Props {
	date: DateItem
}
interface Emit {
	(emit: 'select-date', date: string, dateId: number): void
	(emit: 'select-time', date: string, dateId: number, timeId: number): void
	(emit: 'remove-time', dateId: number, timeId: number): void
	(emit: 'add-time', id: number): void
}

const emit = defineEmits<Emit>()

const props = defineProps<Props>()

const datePickerFormat = ref<'date' | 'time' | null>()
const editedTime = ref<{ id: number; time: string }>()

const selectDate = (date: string) => {
	if (datePickerFormat.value === 'date') {
		emit('select-date', date, props.date.id)
	} else {
		emit('select-time', date, props.date.id, editedTime.value?.id as number)
	}
	datePickerFormat.value = null
}
</script>

<template>
	<div class="date">
		<calendar-input
			:date="date.date"
			@click="datePickerFormat = 'date'"
		/>
		<div class="date__times">
			<calendar-input
				v-for="time in date.times"
				:date="time.time"
				:times-length="date.times.length"
				is-time
				@click=";[(datePickerFormat = 'time'), (editedTime = time)]"
				@remove="emit('remove-time', date.id, time.id)"
			/>
			<ui-button @click="emit('add-time', date.id)">Добавить время</ui-button>
		</div>
	</div>

	<transition>
		<calendar-picker
			v-if="datePickerFormat"
			:date="datePickerFormat === 'date' ? date.date : editedTime?.time as string"
			:format="datePickerFormat"
			@cancel="datePickerFormat = null"
			@select="selectDate"
		/>
	</transition>
</template>

<style scoped lang="scss">
.date {
	display: grid;
	grid-template-columns: repeat(2, 1fr);
	gap: 8px;
	padding: 8px;
	border: 1px solid #646cff;
	border-radius: 8px;

	&__times {
		display: flex;
		flex-direction: column;
		gap: 8px;

		:last-child {
			margin-top: 8px;
		}
	}
}

.v-enter-active,
.v-leave-active {
	transition: opacity 0.25s ease;
}

.v-enter-from,
.v-leave-to {
	opacity: 0;
}
</style>
