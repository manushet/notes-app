<template>
    <section>
        <div class='container'>
            <div class='header'>
                <h1>{{ title }}</h1>
                <search-note placeholder='Type something here...' @search='onSearch'/>
                <div class='icons'>
                    <svg 
                        :class="{ active: grid }"
                        @click='setLayout("grid")'
                        xmlns="http://www.w3.org/2000/svg" 
                        width="24" 
                        height="24" 
                        viewBox="0 0 24 24" 
                        fill="none" 
                        stroke="currentColor" 
                        stroke-width="2" 
                        stroke-linecap="round" 
                        stroke-linejoin="round">
                        <rect x="3" y="3" width="7" height="7"></rect><rect x="14" y="3" width="7" height="7"></rect><rect x="14" y="14" width="7" height="7"></rect><rect x="3" y="14" width="7" height="7"></rect>
                    </svg>
                    <svg 
                        :class="{ active: !grid }"
                        @click='setLayout("column")'
                        xmlns="http://www.w3.org/2000/svg" 
                        width="24" 
                        height="24" 
                        viewBox="0 0 24 24" 
                        fill="none" 
                        stroke="currentColor" 
                        stroke-width="2" 
                        stroke-linecap="round" 
                        stroke-linejoin="round">
                        <line x1="8" y1="6" x2="21" y2="6"></line><line x1="8" y1="12" x2="21" y2="12"></line><line x1="8" y1="18" x2="21" y2="18"></line><line x1="3" y1="6" x2="3" y2="6"></line><line x1="3" y1="12" x2="3" y2="12"></line><line x1="3" y1="18" x2="3" y2="18"></line>
                    </svg>
                </div>
            </div>
        </div>
    </section>            
</template>

<script>
import SearchNote from './SearchNote.vue'

export default {
    name: 'AppHeader',
    components: { SearchNote },
    emits: ['onLayoutChange', 'onSearch'],
    props: {
        grid: {
            type: Boolean,
            default: true
        }
    },
    data: () => ({
        title: 'Notes App',
    }),
    methods: {
        setLayout(type) {
            this.$emit('onLayoutChange', type);
        },
        onSearch(q) {
            this.$emit('onSearch', q);
        }
    }    
}
</script>

<style scoped lang='scss'>
    .header {
        display: flex;
        flex-direction: row;
        flex-wrap: nowrap;
        align-content: center;
        justify-content: space-between;
        align-items: center;    
    }

    h1 {
        text-align: left;
        font-size: 32px;
        font-weight: 500;
    }
    .icons {
        display: flex;
        flex-direction: row;
        justify-content: flex-end;
    }

    .icons > svg {
        cursor: pointer;
        margin-right: 12px;
        &.active {
            color: #402caf;
        }        
        &:last-child {
            margin-right: 0px;
        }

        color: #999;
    }                
</style>