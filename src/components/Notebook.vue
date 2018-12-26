<template>
    <div class="notebook">
        <ul>
            <li v-for="(page, index) of pages" class="page" :key=page.ref 
                :class="{ 'active': index=== activePage }" 
                @click="goPage(index)">
                <div>{{page.title}}</div>
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        name: 'Notebook',
        props: [
            'pages',
            'activelog'
        ],
        
        methods: {
            goPage (index) {
                this. $emit ('go-page', index)
            },
            newPage () {
                this. $emit ('new-page')
            }
        }
    }    
</script>

<style scoped>
    .notebook {
        max-width: 20 rem;
        width: 30 rem;
        background: #ecfeff;
    }
    ul {
        list-style-type: name;
        padding: none;
        margin: 0;
        height: 100%;
        position: relative;
    }
    li {
        padding: 1 rem;
        font-size: 1.25 rem;
        min-height: 1.5 rem;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipses;
    }
    li.hover {
        cursor: pointer;
        background-color: #c5edf5;
    }
    .active {
        background-color: #80eee0;
    }
    .active-hover {
        background-color: #5caec1;
    }
    .new-page {
        background-color: #233b5d;
        color: white;
        position: absolute;
        bottom: 0;
        width: 100%;
        box-sizing: border-box;
    }                
    .new-page-hover {
        background-color: #182c48;
    }
</style>      