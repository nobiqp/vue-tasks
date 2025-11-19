<script lang="ts" setup>
import TaskItem from '~/components/TaskItem.vue';
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";

const itemRefs = ref<InstanceType<typeof TaskItem>[]>([]);

const state = reactive({
	tasks: [] as { id: number; title: string; priority: string; date: string; description: string; }[]
});

onMounted(() => {
	const retrievedString = localStorage.getItem('tasks');
	state.tasks = JSON.parse(retrievedString || '[]');
});

const handleDeleteTask = (taskId: number) => {
	state.tasks = state.tasks.filter(task => task.id !== taskId)
	localStorage.setItem('tasks', JSON.stringify(state.tasks))
}

function animate() {
  gsap.registerPlugin(ScrollTrigger);

  ScrollTrigger.getAll().forEach(t => t.kill());

  itemRefs.value.forEach((itemInstance) => {
    const el = itemInstance.root;
    if (!el) return

    gsap.from(el, {
      x: 2000,
      duration: 1.5,
      scrollTrigger: {
        trigger: el,
        start: "top 90%",
        end: "bottom 90%",
        scrub: false,
      }
    });
  });
}

onUpdated(() => {
  animate();
});

</script>

<template>
	<main class="container container-index">
		<TaskItem v-for="task in state.tasks" ref="itemRefs" :key="task.id" :task="task" class="task-item" @delete="handleDeleteTask" />
	</main>
</template>