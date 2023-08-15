<template>
    <div class='new-note'>
        <error-message :error='error' v-if='error'/>
        <label>Title</label>
        <input 
            type='text' 
            name='new-note-title'
            v-model.trim='note.title'/>
        <div class='radio-group'>
            <div class='radio-item'>  
                <label for='low-priority'>Low Priority</label> 
                <input 
                    type='radio' 
                    id='low-priority'
                    name='new-note-priority'
                    value='low'
                    checked='checked'
                    v-model='note.priority'/>   
            </div>
            <div>            
                <label for='high-priority'>High Priority</label> 
                <input 
                    type='radio' 
                    id='high-priority'
                    name='new-note-priority'
                    value='high'
                    v-model='note.priority'/>   
            </div>
            <div>            
                <label for='urgent-priority'>Urgent Priority</label> 
                <input 
                    type='radio' 
                    id='urgent-priority'
                    name='new-note-priority'
                    value='urgent'
                    v-model='note.priority'/>   
            </div>  
        </div>                             
        <label>Description</label>
        <textarea 
            name='new-note-description'
            v-model.trim='note.description'>
        </textarea>
        <button class="btn btnPrimary" @click='addNote'>New Note</button>
    </div>
</template>

<script>
import ErrorMessage from './ErrorMessage.vue';
export default {
    components: { ErrorMessage },
    name: 'AddNote',
    emits: ['addNewNote'],
    data: () => ({
        note: {
            title: null,
            description: null,
            priority: 'low',
        },
        error: null
    }),  
    methods: {
        addNote() {
            this.error = null;
            if (this.note.title) {
                const id = crypto.randomUUID();
                const date = new Date().toLocaleString();
                this.note = {
                    id,
                    date,
                    ...this.note,
                }
                this.$emit('addNewNote', this.note);
                this.note = {
                    title: null,
                    description: null,
                    priority: 'low',                    
                };
            }
            else {
                this.error = 'You must provide the New Note Title';
            }
        }
    }  
}
</script>

<style scoped>
    .new-note {
        text-align: center;
    }

    .radio-group {
        display: flex;
        width: 100%;
        flex-wrap: wrap;
        flex-direction: row;
        align-content: center;
        justify-content: space-evenly;
        align-items: center;   
    }
             
</style>