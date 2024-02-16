<script setup lang="ts">
import { ref } from 'vue'
import TimetableEditor from './components/Timetable/TimetableEditor.vue'
import TimetablePreview from './components/Timetable/TimetablePreview.vue'
import { IonApp } from '@ionic/vue'
import { DateItem } from './types/timetableTypes'

const isEdit = ref(true)
const selectedDates = ref<DateItem[]>([
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

const onSave = (dates: DateItem[]) => {
	selectedDates.value = dates
	isEdit.value = false
}
</script>

<template>
	<ion-app>
		<timetable-editor
			:dates="selectedDates"
			v-if="isEdit"
			@save="onSave"
		/>
		<timetable-preview
			:dates="selectedDates"
			@edit="isEdit = true"
			v-else
		/>
	</ion-app>
</template>

<style scoped lang="scss"></style>
