<script setup lang="ts">
import { ref } from 'vue'
import VueDatePicker from '@vuepic/vue-datepicker'
import '@vuepic/vue-datepicker/dist/main.css'
//@ts-ignore
import download from 'downloadjs'

const content = ref<HTMLElement>()
const isEdit = ref(true)

const dates = ref([
	{
		id: 1,
		date: new Date(),
		times: [
			{
				id: 1,
				time: {
					hours: new Date().getHours(),
					minutes: new Date().getMinutes()
				}
			}
		]
	}
])

const addTime = (index: number) => {
	dates.value[index].times.push({
		id: dates.value[index].times.length + 1,
		time: {
			hours: new Date().getHours(),
			minutes: new Date().getMinutes()
		}
	})
}
const addDate = () => {
	dates.value.push({
		id: dates.value.length + 1,
		date: new Date(),
		times: [
			{
				id: 1,
				time: {
					hours: new Date().getHours(),
					minutes: new Date().getMinutes()
				}
			}
		]
	})
}

interface TimeObj {
	hours: number
	minutes: number
}

const getDate = (date: Date) => {
	return date.toLocaleString('ru', {
		day: '2-digit',
		month: 'long'
	})
}

const getWeek = (date: Date) => {
	return date.toLocaleString('ru', {
		weekday: 'long'
	})
}

const getDay = (date: Date) => {
	return date.toLocaleString('ru', {
		day: 'numeric'
	})
}

const getTime = ({ hours, minutes }: TimeObj) => {
	let min = '0'

	if (minutes.toString().length > 1) {
		min = minutes.toString()
	} else {
		min = minutes > 9 ? minutes.toString() : `0${minutes}`
	}
	return `${hours > 9 ? hours : `0${hours}`}:${min}`
}

const buttonsVisible = ref(true)

const saveImage = async () => {
	buttonsVisible.value = false

	setTimeout(() => {
		buttonsVisible.value = true
	}, 7000)

	// nextTick(async () => {
	// 	if (content.value) {
	// 		const img = await toPng(content.value, {
	// 			imagePlaceholder:
	// 				'https://media.discordapp.net/attachments/1021492073880371370/1202684450878791691/photo_2024-01-20_22-37-00.jpg?ex=65ce5a2d&is=65bbe52d&hm=52b0f1c3b4f52b37c35734bc3ead9daf3718896b0626ea7bb388c86d0b9c63d6&=&format=webp&width=487&height=866'
	// 		})

	// 		await download(img, 'image.png')

	// 		buttonsVisible.value = true
	// 	}
	// })
}
</script>

<template>
	<div
		v-if="isEdit"
		class="inputs"
	>
		<div
			v-for="(group, groupIndex) in dates"
			:key="group.id"
			class="inputs__input-date input-date"
		>
			<div>
				<vue-date-picker
					v-model="group.date"
					locale="ru"
				/>
				<div style="margin-top: 16px; font-family: 'Montserrat', sans-serif">
					{{ getDate(group.date) }}
				</div>
			</div>
			<ul class="input-date__list">
				<li v-for="time in group.times">
					<vue-date-picker
						v-model="time.time"
						locale="ru"
						time-picker
					/>
				</li>
				<button @click="addTime(groupIndex)">Добавить время</button>
			</ul>
		</div>
		<button @click="addDate">Добавить дату</button>
		<button
			style="margin-top: auto"
			@click="isEdit = false"
		>
			Сохранить
		</button>
	</div>
	<div
		v-else
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
				<button
					@click="isEdit = true"
					style="width: 100%; background-color: white; color: #646cff"
				>
					Редактировать
				</button>
				<button
					@click="saveImage"
					style="width: 100%"
				>
					Сделать скриншот
				</button>
			</div>
		</div>
	</div>
</template>

<style scoped lang="scss">
.inputs {
	display: flex;
	flex-direction: column;
	gap: 16px;
	height: 100%;
	padding: 1rem;

	.input-date {
		display: grid;
		grid-template-columns: repeat(2, 1fr);
		gap: 8px;
		padding: 8px;
		border: 1px solid #646cff;
		border-radius: 8px;

		&__list {
			display: flex;
			flex-direction: column;
			gap: 8px;
		}
	}
}

.content {
	height: 100%;
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
		font-size: 60px;
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
					font-size: 16px;
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
			}
		}
	}
}
</style>
