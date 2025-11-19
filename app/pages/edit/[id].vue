<script lang="ts" setup>
const route = useRoute()

const handleTaskForm = (payload: {id: number; title: string; priority: string; date: string; description: string; }) => {
	const retrievedString = localStorage.getItem('tasks');
	const retrievedObjects = JSON.parse(retrievedString || '[]');

	const taskIndex = retrievedObjects.findIndex((task: { id: number }) => task.id === Number(route.params.id));
	if (taskIndex !== -1) {
		retrievedObjects[taskIndex] = { ...retrievedObjects[taskIndex], ...payload };
		localStorage.setItem('tasks', JSON.stringify(retrievedObjects));
	}
	navigateTo(`/tasks/${route.params.id}`);
};

</script>

<template>
	<TaskInput :id="String(route.params.id)" title="Edit Task" @task-form="handleTaskForm"/>
</template>