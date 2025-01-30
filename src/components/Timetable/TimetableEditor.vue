<script setup lang="ts">
import TimetableDate from './TimetableDate.vue'
import { DateItem } from '@/types/timetableTypes'
import UiButton from '../UI/UiButton.vue'
import { ref } from 'vue'
import { klona } from 'klona/lite'

interface Emit {
	(emit: 'save', dates: DateItem[]): void
}

interface Props {
	dates: DateItem[]
}

const emit = defineEmits<Emit>()
const props = defineProps<Props>()

const dates = ref<DateItem[]>(klona(props.dates))

const addTime = (id: number) => {
	const date = dates.value.find((el) => el.id === id)

	if (date) {
		date.times.push({
			id: new Date().getMilliseconds(),
			time: new Date().toISOString()
		})
	}
}
const addDate = () => {
	dates.value.push({
		id: new Date().getMilliseconds(),
		date: new Date().toISOString(),
		times: [
			{
				id: new Date().getMilliseconds(),
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

const onRemoveDate = (id: number) => {
    dates.value = dates.value.filter(date => date.id !== id);
}
</script>

<template>
	<div class="timetable">
        <transition-group class="timetable__list" name="list" tag="ul">
            <li
                v-for="date in dates"
                :key="date.id"
            >
                <timetable-date
                    :date="date"
                    @select-date="editDate"
                    @select-time="editTime"
                    @add-time="addTime"
                    @remove-time="removeTime"
                    @remove-date="onRemoveDate"
                />
            </li>
            <ui-button @click="addDate" style="width: 100%">Добавить дату</ui-button>
        </transition-group>

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
	overflow-y: scroll;
	padding: 36px 16px 16px;
    display: flex;
    flex-direction: column;

    &__list {
        display: flex;
        flex-direction: column;
        gap: 1rem;
    }
}

.list-move, /* apply transition to moving elements */
.list-enter-active,
.list-leave-active {
    transition: all 0.3s ease-in-out;
}

.list-enter-from,
.list-leave-to {
    opacity: 0;
    transform: translateY(-1.5rem);
}

/* ensure leaving items are taken out of layout flow so that moving
   animations can be calculated correctly. */
.list-leave-active {
    position: absolute;
}
</style>
