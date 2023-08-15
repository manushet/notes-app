<template>
    <div class="wrapper">
        <div class='wrapper-content'>
            <section>
                <div class='container'>
                    <add-note @addNewNote='onAddNewNote'/>
                </div>
            </section>
            <app-header 
                :grid='grid' 
                @onLayoutChange='onLayoutChange'
                @onSearch='onSearch'/>
            <section>
                <div class='container'>
                    <notes-list 
                        :notes='filteredNotes' 
                        :grid='grid' 
                        @onRemoveNote='onRemoveNote'
                        @onEditNote='onEditNote'/>
                </div>
            </section>             
        </div>
    </div>
</template>

<script>
import AddNote from './components/AddNote.vue';
import AppHeader from './components/AppHeader.vue';
import NotesList from './components/NotesList.vue';

export default {
    name: 'App',
    components: {
        NotesList,
        AddNote,
        AppHeader,
    },
    data: () => ({
        notes: [
            {
                id: 1,
                title: 'Note #1',
                description: 'Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry`s standard dummy text ever since the 1500s.',
                date: '01.08.2023 09:51:11',
                priority: 'low',
            },
            {
                id: 2,
                title: 'Note #2',
                description: 'It is a long established fact that a reader will be distracted by the readable content of a page when looking at its layout.',
                date: '15.04.2023 13:51:36',
                priority: 'high',
            },
            {
                id: 3,
                title: 'Note #3',
                description: 'The point of using Lorem Ipsum is that it has a more-or-less normal distribution of letters, as opposed to using "Content here, content here", making it look like readable English.',
                date: '26.02.2023 09:18:07',
                priority: 'urgent',
            }
        ],
        grid: true,
        search: null,
    }),
    computed: {
        filteredNotes() {
            if (this.search) {
                return this.notes.filter(item => item.title.toLocaleLowerCase().indexOf(this.search) !== -1);
            }
            return this.notes;
        }
    },
    methods: {
        onAddNewNote(note) {
            console.log(note);
            this.notes.push(note);
        },
        onRemoveNote(id) {
            this.notes = this.notes.filter(el => el.id !== id);
            console.log('Remove ', id);
        },
        onLayoutChange(type) {
            if (type === 'grid') {
                this.grid = true;
            }
            else {
                this.grid = false;
            }
        },
        onSearch(q) {
            this.search = q;
        },
        onEditNote(note) {
            this.notes = this.notes.map(item => {
                if ((item.id).toString() === (note.id).toString()) {
                    const title = note.newTitle || item.title;
                    const description = note.newDescription || item.description;
                    const date = note.newDate || item.date;
                    return {
                        ...item,
                        title,
                        description,
                        date,
                    };
                }
                return item;
            });
        },           
    }
}
</script>

<style lang="scss" scoped>    

</style>
