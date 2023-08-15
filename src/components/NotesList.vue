<template>
    <div>
        <div class='notes'>
            <div class='note' 
                :class='{ [note.priority]: true, full: !grid }'
                v-for='(note, i) in notes'
                :key='note.id'>
                <div class='note-header'>
                    <input  
                        v-if='editTitle[note.id]'
                        :ref='"input" + i'
                        type='text' 
                        :name='"new-note-title-" + note.id'
                        :value='note.title'
                        :data-id='note.id'
                        data-edit-field='true'
                        :key='note.id'/>                    
                    <p v-else @click.stop='editTitleMode(note.id)'>{{ note.title }}</p>
                    <p style='cursor: pointer' @click='removeNote(note.id)'>x</p>
                </div>
                <div class='note-body'>
                    <textarea  
                        v-if='editDescription[note.id]'
                        :ref='"textarea" + i'
                        :name='"new-note-title-" + note.id'
                        :data-id='note.id'
                        data-edit-field='true'
                        :value='note.description'
                        :key='note.id'>
                    </textarea>                      
                    <p v-else @click.stop='editDescriptionMode(note.id)'>{{ note.description }}</p>
                    <span>{{ note.date }}</span>
                </div>                
            </div>            
        </div>
    </div>
</template>

<script>
export default {
    name: 'NotesList',
    emits: ['onRemoveNote', 'onEditNote'],
    props: {
        notes: {
            type: Array,
            required: true,
        },
        grid: {
            type: Boolean,
            default: true
        },
    },
    data: () => ({
        newTitle: null,
        newDescription: null,
        editTitle: {},
        editDescription: {}
    }),
    created() {
        this.editTitle = this.notes.reduce((acc, item) => {
            acc[item.id] = false;
            return acc;
        }, {});
    },
    mounted() { 
        document.addEventListener('click', this.onClickOutside);
        document.addEventListener('keyup', this.onKeyUp);
    },
    unmounted() { 
        document.removeEventListener('click', this.onClickOutside);
        document.removeEventListener('keyup', this.onKeyUp);
    },
    computed: {
    },
    methods: {
        removeNote(id) {
            this.$emit('onRemoveNote', id);
        },
        editTitleMode(id) {
            this.editTitle[id] = true;
        },
        editDescriptionMode(id) {
            this.editDescription[id] = true;
        },        
        onClickOutside(e) {
            if (e.target.getAttribute("data-edit-field")) {
                return;
            }
            else {
                this.closeTitleInputs();
                this.closeDescriptionInputs();
                this.saveInputs();
            }
        },
        onKeyUp(e) {
            if (e.key === 'Escape') {
                this.closeTitleInputs();
                this.closeDescriptionInputs();
            }
            else if (e.key === 'Enter') {
                this.closeTitleInputs();                
                this.closeDescriptionInputs();
                this.saveInputs();                
            }  
            else return;          
        },
        saveInputs() {
            Object.keys(this.$refs).forEach(key => {
                if (this.$refs[key].length) {
                    let newTitle = null;
                    let newDescription = null;
                    
                    if (key.includes('textarea')) {
                        newDescription = this.$refs[key][0].value;
                    }
                    else {
                        newTitle = this.$refs[key][0].value;
                    }
                    const note = {
                        id: this.$refs[key][0].getAttribute("data-id"),
                        newTitle,
                        newDescription,
                        newDate: new Date().toLocaleString(),
                    };
                    this.$emit('onEditNote', note);
                }
            });
        },        
        closeTitleInputs() {
            Object.keys(this.editTitle).forEach(id => {
                this.editTitle[id] = false;
            });
        },
        closeDescriptionInputs() {
            Object.keys(this.editDescription).forEach(id => {
                this.editDescription[id] = false;
            });
        },        
    }
}
</script>

<style scoped lang="scss">
    @import "../assets/scss/utils/vars.scss"; 
    .notes {
        display: flex;
        align-items: center;
        justify-content: space-between;
        flex-wrap: wrap;
        padding: 40px 0;
    }

    .note {
        display: flex;
        align-items: center;
        justify-content: left;        
        flex-direction: column;
        width: 48%;
        height: 200px;
        padding: 18px 20px;
        margin-bottom: 20px;
        background-color: #ffffff;

        transition: all .25s cubic-bezier(.025, .01, .50, 1);
        box-shadow: 0 30px 30px rgba(0, 0, 0, .025);

        &:hover {
            box-shadow: 0 30px 30px rgba(0, 0, 0, .04);
            transform: translate(0, -5px);
            transition-delay: 0s !important;
        }

        &.full {
            width: 100%;
        }
        &.low {
            border-left: 5px solid #402caf;
            .note-header {  
                p {
                    color: #402caf;
                }  
            }             
        }    
        &.high {
            border-left: 5px solid #ff8d00;
            .note-header {  
                p {
                    color: #ff8d00;
                }  
            }              
        } 
        &.urgent {
            border-left: 5px solid #ff000c;
            .note-header {  
                p {
                    color: #ff000c;
                }  
            }              
        }                          
    }

    .text-content {
        width: 100%;
    }   

    .note-header {
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: space-between;        
        p {
            font-size: 22px;
            cursor: text;
        }
        &.full {
            justify-content: center;
            p {
                margin-right: 16px;
                &:last-child {
                    margin-right: 0;
                }
            }
        }

        &.input {
            margin-right: 1em;
        }
    }

    .note-body {
        width: 100%;
        margin-top: 0.75em;
        p {
            margin: 20px 0;
            max-height: 50px;
            overflow-x: hidden;
            overflow-y: auto;
        }

        span {
            font-size: 14px;
            color: #999;
        }
        .note-editor {
            margin: 20px 0;
            max-height: 50px;
            overflow-x: hidden;
            overflow-y: auto;

            font-weight: normal;
            color: black;
        }        
    }

    .note-editor {
        height: auto;
        padding: 0;
        margin-bottom: 0;
        border-radius: 5px;

        font-family: "Montserrat", Helvetica, Arial, sans-serif;
        font-size: 100%;
        font-weight: bold;
        color: #494ce8;
    }  
    
    .close-btn {
        color: #494ce8;
        background-color: white;
        border: none;
        cursor: pointer;
    }
    
    .priority {
        width: 15px;
        height: 110px;
        margin-right: 30px;

        border-radius: 20px;
        background-color: $success-color;

        &.high {
            background-color: $danger-color;
        }

        &.low {
            background-color: $primary-color;
        }
    }    

</style>
