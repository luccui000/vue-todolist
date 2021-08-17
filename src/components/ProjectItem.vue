<template>
    <div class="project" :class="{ completed: project.completed }">
        <div v-if="!edit" class="content " @click="showDetails = !showDetails">
            <h4 class="title" >{{ project.title }}</h4>
            <p v-if="showDetails">{{ project.details }}</p>
        </div>   
        <div v-else class="content"  @click="showDetails = !showDetails" >
            <label for="" class="group-text">
                <span>Title</span>
                <input 
                    type="text" v-model="title" class="input-text">
            </label> 
            <label for="" class="group-text">
                <span>Detail</span>
                <textarea 
                    type="text" 
                    v-model="details" 
                    class="input-text">
                </textarea>
            </label> 
        </div>
        <div class="icon">
            <span class="material-icons" @click="edit = !edit" v-if="!edit">mode</span> 
            <div v-else class="editing">
                <span class="material-icons" @click="saveEdited(project.id)" >inventory</span>   
                <span class="material-icons" @click="edit = !edit" >cancel</span>   
            </div>
            <span class="material-icons" @click="completedProject(project.id)">done</span> 
            <span class="material-icons" @click="deleteProject(project.id)">delete</span> 
        </div>
    </div>
</template>

<script>
import { PROJECTS_ENDPOINT } from '../constants'
export default {
    props: ['project'],
    data() {
        return {
            showDetails: false,
            edit: false,
            title: this.project.title || '',
            details: this.project.details || '', 
        }
    },
    methods: {
        saveEdited(id) {
            let body = JSON.stringify({ title: this.title, details: this.details });
            this.patchProject(id, body);
            this.$emit('edited', id, body);
        },
        completedProject(id) {
            this.patchProject(id, JSON.stringify({ completed: !this.project.completed }))
            this.$emit('completed', id);
        },
        async deleteProject(id) {
            await fetch(`${PROJECTS_ENDPOINT}/${id}`, { method: 'DELETE'});
            this.$emit('deleted', id);
        },
        async patchProject(id, body) {
            await fetch(`${PROJECTS_ENDPOINT}/${id}`, { 
                method: 'PATCH',  
                headers: { 'Content-Type': 'application/json' },
                body: body
            })
            this.edit = this.showDetails = false; 
            alert("Saved"); 
        }
    }
}
</script>

<style scoped>
    .project {
        display: flex;
        padding: 0px 20px;
        margin: 10px;
        justify-content: space-between;
        align-items: center;
        background-color: rgb(236, 236, 236);
        border-radius: 5px; 
        border-left: 4px solid rgb(253, 122, 122);
        text-align: left;
    }
    .completed {
        border-left: 4px solid rgb(12, 209, 117);
    } 
    .title {
        cursor: pointer; 
    }
    .material-icons {
        cursor: pointer; 
        margin-left: 10px;
    }
    .input-text {
        display: block;
        width: 300px;
        margin: 10px 10px;
    }
    .group-text {
        display: flex; 
        justify-content: space-between;
        align-items: center;
    }
    .group-text span {
        width: 60px;
        font-size: 14px;
    }
    .input-text {
        flex: 1;
        border: 1px dashed #ccc;
        padding: 4px;
    }
    .input-text:focus { 
        background-color: rgb(243, 243, 243);
    }
    .editing {
        display: inline-block;
    } 
</style>