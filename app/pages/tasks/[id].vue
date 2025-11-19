<script lang="ts" setup>
import '~/assets/css/tasks.css'
const route = useRoute()

const form = reactive({
	title: '',
	date: '',
	priority: "high",
	description: ''
});

const formattedDate = computed(() => {
  if (!form.date) return '';
  return new Date(form.date).toLocaleDateString('en-US', {
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  });
});

onMounted(() => {
		const retrievedString = localStorage.getItem('tasks');
		const retrievedObjects = JSON.parse(retrievedString || '[]');
		const taskToEdit = retrievedObjects.find((task: { id: number }) => task.id === Number(route.params.id));
		if (taskToEdit) {
			form.title = taskToEdit.title;
			form.date = taskToEdit.date;
			form.priority = taskToEdit.priority;
			form.description = taskToEdit.description;
		}
});
</script>

<template>
	<main class="container container-tasks">
		<div class="tasks-header">
			<h2>{{form.title}}</h2>
			<NuxtLink class="link btn" to="/">Go Back</NuxtLink>
		</div>
		<div class="tasks-content">
			<div calss="tasks-priority">
				Priority: <span :class="`priority capitalize ${form.priority}`" >{{ form.priority }}</span>
			</div>
			<div calss="tasks-date">
				Due date on: <span>{{formattedDate}}</span>
			</div>
			<div class="tasks-description">
				<p>
					{{ form.description }}
				</p>
			</div>
		</div>
	</main>
</template>