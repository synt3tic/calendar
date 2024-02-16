<script setup lang="ts">
import TimetableDate from './TimetableDate.vue'
import { DateItem } from '../../types/timetableTypes'
import UiButton from '../UI/UiButton.vue'
import { ref } from 'vue'

interface Emit {
	(emit: 'save', dates: DateItem[]): void
}

const emit = defineEmits<Emit>()

const dates = ref<DateItem[]>([
	{
		id: 1,
		date: new Date().toISOString(),
		times: [
			{
				id: 1,
				time: new Date().toISOString()
			}
		]
	}
])

const addTime = (id: number) => {
	const date = dates.value.find((el) => el.id === id)

	if (date) {
		date.times.push({
			id: date.times.length + 1,
			time: new Date().toISOString()
		})
	}
}
const addDate = () => {
	dates.value.push({
		id: dates.value.length + 1,
		date: new Date().toISOString(),
		times: [
			{
				id: 1,
				time: new Date().toISOString()
			}
		]
	})
}

const editDate = (date: string, id: number) => {
	const findedDate = dates.value.find((el) => el.id === id)

	findedDate && (findedDate.date = date)
}

const editTime = (date: string, dateId: number, timeId: number) => {
	const findedDate = dates.value.find((el) => el.id === dateId)

	if (findedDate) {
		const findedTime = findedDate.times.find((el) => el.id === timeId)

		findedTime && (findedTime.time = date)
	}
}

const removeTime = (dateId: number, timeId: number) => {
	const findedDate = dates.value.find((el) => el.id === dateId)

	if (findedDate && findedDate.times.length > 1) {
		findedDate.times = findedDate.times.filter((el) => el.id !== timeId)
	}
}
</script>

<template>
	<div class="timetable">
		<timetable-date
			v-for="date in dates"
			:key="date.id"
			:date="date"
			@select-date="editDate"
			@select-time="editTime"
			@add-time="addTime"
			@remove-time="removeTime"
		/>

		<ui-button @click="addDate">Добавить дату</ui-button>
		<ui-button
			@click="emit('save', dates)"
			style="margin-top: auto"
		>
			Сохранить
		</ui-button>
	</div>
</template>

<style scoped lang="scss">
.timetable {
	height: 100dvh;
	padding: 36px 16px 16px;
	display: flex;
	flex-direction: column;
	gap: 8px;
}
</style>
