<template>
	
	<!-- projects start -->
	<div class="project-section" id="projects">

		<div class="container-fluid d-none d-md-block">
			<div class="row" id="h1-projects">
				<h1>PROJECTS</h1>
			</div>
			<div class="row" id="row-projects">

				<div 
				    v-for="(group, index) in chunkedProjects" 
				    :key="index" 
				    class="row my-5 justify-content-center"
				>

				<ProjectCard 
				    v-for="project in group" 
				    :key="project.id" 
				    :project="project"
				/>


				</div>
			</div>
		</div>
	</div>
		
</template>

<script setup>
	
	import { computed } from 'vue';
	import ProjectCard from './ProjectCard.vue';
	import projects from '../data/projects.json';

	const chunkSize = 3;


	const chunkedProjects = computed(() => {
	  const chunks = [];
	  for (let i = 0; i < projects.length; i += chunkSize) {
	    chunks.push(projects.slice(i, i + chunkSize));
	  }
	  return chunks;
	});
	

</script>