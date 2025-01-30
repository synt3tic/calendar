<script setup lang="ts">
import { ref } from 'vue'
import TimetableEditor from './components/Timetable/TimetableEditor.vue'
import TimetablePreview from './components/Timetable/TimetablePreview.vue'
import { IonApp } from '@ionic/vue'
import { DateItem } from './types/timetableTypes'

const isEdit = ref(true);
const selectedDates = ref<DateItem[]>([
	{
		id: new Date().getMilliseconds(),
		date: new Date().toISOString(),
		times: [
			{
				id: new Date().getMilliseconds(),
				time: new Date().toISOString()
			}
		]
	},
])

const onSave = (dates: DateItem[]) => {
	selectedDates.value = dates
	isEdit.value = false
}
</script>

<template>
	<ion-app>
		<timetable-editor
            v-if="isEdit"
			:dates="selectedDates"
			@save="onSave"
		/>
		<timetable-preview
            v-else
			:dates="selectedDates"
			@edit="isEdit = true"
		/>
	</ion-app>
</template>

<style scoped lang="scss"></style>
