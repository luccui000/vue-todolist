<template>
	<div class="home">   
		<div class="round">
			<button class="btn" @click="showAddNew = !showAddNew">Add New</button>  
		</div> 
		<AddNewProject 
			v-if="showAddNew" 
			@addNewProject="handleAddNewProject"
		/>
		<div v-for="project in projects" :key="project.id">
			<ProjectItem 
				:project="project" 
				@edited="handleEdited" 
				@completed="handleCompleted"
				@deleted="handleDeleted"
			/>
		</div>
		<div class="">
			<span>Done: {{ completedProjectCounter }}, </span>
			<span>On going: {{ onGoingProjectCounter }}</span> 
		</div>
	</div>
</template>

<script> 
import ProjectItem  from '../components/ProjectItem.vue'
import AddNewProject from '../components/AddNewProject.vue'
import { PROJECTS_ENDPOINT } from '../constants'

export default {
	components: {
		ProjectItem,
		AddNewProject
	}, 
	data() {
		return {
			showAddNew: false,
			projects: []
		}
	},
	created() {
		fetch(`${PROJECTS_ENDPOINT}`)
			.then(res => res.json())
			.then(data => this.projects = data) 
	},
	methods: {
		handleEdited(id, body) {
			let project = this.findProject(id)
			if(project) {
				let data = JSON.parse(body);
				project.title = data.title;
				project.details = data.details;
			}
		},
		handleCompleted(id) {
			let project = this.findProject(id)
			if(project) {
				project.completed = !project.completed;
			}
		},
		handleDeleted(id) {
			let project = this.findProject(id) 
			if(project) {
				this.projects = this.projects.filter(e => e.id !== id)
			}
		},
		handleAddNewProject(body) {
			let lastIndex = this.projects[this.projects.length - 1];
			let project = JSON.parse(body);

			project.id = lastIndex;
			this.projects.push(project)
		},
		findProject(id) {
			return this.projects.find(e => e.id === id) || null;
		}
	},
	computed: {
		completedProjectCounter() {
			return this.projects.filter(e => e.completed === true).length
		}, 
		onGoingProjectCounter(){
			return this.projects.filter(e => e.completed === false).length
		}
	}
}
</script>
<style scoped>
	.home {
		width: 600px;
		margin: 40px auto;
		box-shadow: 1px 2px 3px #ddd;
		padding: 10px;
	}
	.round {
		width: 100%;
		height: 40px;
		position: relative;
	}
	.btn {
		width: 100px;
		height: 30px;
		background-color: rgb(12, 209, 117);
		color: #fff;
		border: 1px solid transparent;
		border-radius: 10px;
		cursor: pointer; 
		position: absolute;
		right: 10px;
	}
</style>
