<template>
	
	<div class="col-6 col-md-6 col-lg-4">
		<div class="inner-projects">
			<img :src="project.image">
			<h3>{{project.title}}</h3>
			<p>{{project.description}}</p>
			<a href="https://github.com/" id="view-repository" target="_blank">VIEW REPOSITORY &#8599;</a>
		</div>
	</div>

</template>

<script setup>
	
	import { defineProps } from 'vue';

	defineProps({
		project: Object
	})

</script>