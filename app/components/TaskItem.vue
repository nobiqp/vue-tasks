<script lang="ts" setup>
import '~/assets/css/task.css'

const props = defineProps<{ task: { id: number; title: string; priority: string; date: string; description: string; } }>()

const root = ref(null);
defineExpose({ root });

const formattedDate = computed(() => {
	if (!props.task.date) return '';
	return new Date(props.task.date).toLocaleDateString('en-US', {
		year: 'numeric',
		month: 'short',
		day: 'numeric'
	});
});

const emit = defineEmits<{
	(e: 'delete', taskId: number): void
}>()

const handleEdit = () => {
	navigateTo(`/edit/${props.task.id}`);
};

const handleDelete = () => {
	emit('delete', props.task.id);
};

</script>

<template>
	<div ref="root" class="root">
		<div class="container-task">
			<div class="task-header">
				<h2>{{ task.title }}</h2>
			</div>
			<div class="task-details">
				<div class="task-priority">
					<h2>Priority: <span :class="`priority capitalize ${task.priority}`">{{ task.priority }}</span></h2>
				</div>
				<div class="task-date">
					<h2>Due date on: <span>{{ formattedDate }}</span></h2>
				</div>
			</div>
			<div class="menu-wrapper">
				<button class="menu-btn">⋮</button>

				<div class="menu-dropdown">
					<button class="menu-item" @click="handleEdit">Edit</button>
					<button class="menu-item delete" @click="handleDelete">Delete</button>
				</div>
			</div>

		</div>
	</div>
</template>
