<template>
    <div class="new-project">
        <label for="" class="group-input">
            <span>Title</span>
            <input 
                type="text" 
                v-model="title" 
                class="input-text" 
            />
        </label> 
        <label for="" class="group-input">
            <span>Detail</span>
            <textarea 
                type="text" 
                v-model="details" 
                class="input-text">
            </textarea>
        </label> 
        <button class="btn" @click="addNewProject">Save</button>
    </div>    
</template>

<script>
import { PROJECTS_ENDPOINT } from '../constants'
export default {
    data() {
        return {
            title: '',
            details: ''
        }
    },
    methods: {
        addNewProject() {
            let body = JSON.stringify({ 
                title: this.title, 
                details: this.details,  
                completed: false
            });
            fetch(`${PROJECTS_ENDPOINT}`, { 
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: body
            }).then((res) => { 
                this.$emit('addNewProject', body);
            })
        }
    }
}
</script>

<style scoped>
.new-project {
    display: flex;
    flex-direction: column;
    width: 80%; 
    margin: 0 auto;
}
.group-input {
    display: flex;
    width: 100%;
    position: relative;
    margin: 10px;
}
.group-input span {
    position: absolute;
    top: -10px;
    background-color: #fff;
    left: 10px;
}
.input-text {
    width: 100%;
    padding: 10px;
    border: 2px dashed #ddd;
    border-radius: 2px; 
}
.input-text:focus { 
    outline: none;
    -webkit-box-shadow: none;
    border: 2px dashed #000;
	box-shadow: none;
}
.btn { 
    background-color: rgb(12, 209, 117);
    color: #fff;
    border: 1px solid transparent;
    border-radius: 5px;
    cursor: pointer; 
    margin-left: 15px;
    padding: 5px; 
}
</style>