<script lang="ts" setup>

const handleTaskForm = (payload: {id: number; title: string; priority: string; date: string; description: string; }) => {
	const retrievedString = localStorage.getItem('tasks');
	const retrievedObjects = JSON.parse(retrievedString || '[]');

	let newId = 1;
	if (retrievedObjects.length > 0) {
		const lastObject = retrievedObjects[retrievedObjects.length - 1];
		newId = lastObject.id + 1;
	}
	payload.id = newId;

	retrievedObjects.push(payload);
	localStorage.setItem('tasks', JSON.stringify(retrievedObjects));
	navigateTo(`/tasks/${newId}`);
}
</script>

<template>
	<TaskInput title="Add New Task" @task-form="handleTaskForm"/>
</template>