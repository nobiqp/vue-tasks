<script lang="ts" setup>
import '~/assets/css/header.css'

const route = useRoute()
</script>

<template>
	<header class="header-bar bar">
		<div class="container header-inner">
			<NuxtLink class="link" to="/">
				<h1>Task Cat</h1>
			</NuxtLink>
			<NuxtLink v-if="route.path !== `/add` && route.path !== `/edit`" class="add-button link btn" to="/add">Add Task</NuxtLink>
		</div>
	</header>

</template>