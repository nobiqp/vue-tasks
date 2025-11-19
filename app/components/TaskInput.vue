<script lang="ts" setup>
import '~/assets/css/add.css'

const props = defineProps<{ title: string, id?: string }>()

const emit = defineEmits(['task-form']);

const form = reactive({
	title: '',
	date: '',
	priority: "high",
	description: ''
});

const handleTask = () => {
	emit('task-form', { ...form });
}

onMounted(() => {
	if (props.id) {
		const retrievedString = localStorage.getItem('tasks');
		const retrievedObjects = JSON.parse(retrievedString || '[]');
		const taskToEdit = retrievedObjects.find((task: { id: number }) => task.id === Number(props.id));
		if (taskToEdit) {
			form.title = taskToEdit.title;
			form.date = taskToEdit.date;
			form.priority = taskToEdit.priority;
			form.description = taskToEdit.description;
		}
	}
});
</script>

<template>
	<main class="container container-add">
		<div class="add-header">
			<h2>{{ title }}</h2>
			<NuxtLink class="link btn" to="/">Go Back</NuxtLink>
		</div>
		<form class="add-form" @submit.prevent="handleTask">
			<div>
				<label>Title
					<input id="title" v-model="form.title" class="input" type="text" name="title" required/>
				</label>
			</div>
			<div>
				<label>Due Date
					<input id="date" v-model="form.date" class="input" type="date" name="date" required/>
				</label>
			</div>
			<div>
				<label>Priority
					<div class="priority-options">
						<label>
							<input v-model="form.priority" type="radio" name="priority" value="high" required/>
							High
							<span class="dot priority high"></span>
						</label>
						<label>
							<input v-model="form.priority" type="radio" name="priority" value="moderate"/>
							Moderate
							<span class="dot priority moderate"></span>
						</label>
						<label>
							<input v-model="form.priority" type="radio" name="priority" value="low"/>
							Low
							<span class="dot priority low"></span>
						</label>
					</div>
				</label>

			</div>
			<div>
				<label>Task Description
					<textarea 
					id="description" v-model="form.description" class="input" name="description" rows="4"
						placeholder="Start writing here..." required></textarea>
				</label>
			</div>

			<button class="btn" type="submit">Done</button>
		</form>
	</main>
</template>