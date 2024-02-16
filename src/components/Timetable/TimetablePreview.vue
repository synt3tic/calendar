<script setup lang="ts">
import UiButton from '../UI/UiButton.vue'
import { DateItem } from '@/types/timetableTypes'
import { ref } from 'vue'

interface Props {
	dates: DateItem[]
}

interface Emit {
	(emit: 'edit'): void
}

defineProps<Props>()
const emit = defineEmits<Emit>()

const buttonsVisible = ref(true)

const getDate = (date: string) => {
	let res = ''

	if (date) {
		new Date(date).toLocaleString('ru', {
			day: '2-digit',
			month: 'long'
		})
	}
	return res
}

const getWeek = (date: string) => {
	return new Date(date).toLocaleString('ru', {
		weekday: 'long'
	})
}

const getDay = (date: string) => {
	return new Date(date).toLocaleString('ru', {
		day: 'numeric'
	})
}

const getTime = (date: string) => {
	return new Date(date).toLocaleTimeString('ru', {
		hour: '2-digit',
		minute: '2-digit'
	})
}

const saveImage = async () => {
	buttonsVisible.value = false

	setTimeout(() => {
		buttonsVisible.value = true
	}, 7000)
}
</script>

<template>
	<div
		class="content"
		ref="content"
	>
		<div class="content__wrapper">
			<div class="content__header">
				<p>ДАТА</p>
				<p>ВРЕМЯ</p>
			</div>
			<div class="cards">
				<div
					v-for="card in dates"
					class="cards__item"
				>
					<div class="image">
						<p class="image__week">{{ getWeek(card.date) }}</p>
						<p class="image__day">{{ getDay(card.date) }}</p>
					</div>
					<div class="times">
						<p
							v-for="time in card.times"
							:key="time.id"
						>
							{{ getTime(time.time) }}
						</p>
					</div>
				</div>
			</div>
			<div
				v-if="buttonsVisible"
				style="display: flex; gap: 16px; width: 100%"
			>
				<ui-button
					@click="emit('edit')"
					style="width: 100%; background-color: white; color: #646cff"
				>
					Редактировать
				</ui-button>
				<ui-button
					@click="saveImage"
					style="width: 100%"
				>
					Сделать скриншот
				</ui-button>
			</div>
		</div>
	</div>
</template>

<style scoped lang="scss">
.content {
	height: 100dvh;
	overflow-y: scroll;
	background-image: url('/bg.JPEG');

	&__wrapper {
		display: flex;
		flex-direction: column;
		height: 100%;
		width: 100%;
		padding: 2rem;
		padding-top: 6rem;

		:last-child {
			margin-top: auto;
		}
	}

	&__header {
		font-family: 'Alumni Sans', sans-serif;
		font-weight: 100;
		font-size: 40px;
		color: white;
		width: 100%;
		display: flex;
		justify-content: space-between;

		:first-child {
			letter-spacing: 2px;
		}
	}

	.cards {
		display: flex;
		flex-direction: column;
		margin-top: 36px;
		gap: 24px;
		width: 100%;

		&__item {
			display: flex;
			align-items: center;
			justify-content: space-between;
			width: 100%;

			.image {
				display: flex;
				flex-direction: column;
				padding: 8px;
				background-color: white;
				border-radius: 8px;
				min-width: 94px;
				width: 94px;
				height: 94px;

				font-family: 'Roboto', sans-serif;

				&__week {
					color: rgb(168, 0, 0);
					font-size: 12px;
					font-weight: 500;
					text-transform: capitalize;
				}

				&__day {
					color: black;
					font-size: 32px;
				}
			}

			.times {
				display: flex;
				flex-wrap: wrap;
				justify-content: end;
				align-items: center;
				width: 100%;
				gap: 8px;
				font-family: 'Roboto', sans-serif;
				font-size: 24px;
				margin: 0;
				margin-left: 24px;

				color: white;
			}
		}
	}
}
</style>
