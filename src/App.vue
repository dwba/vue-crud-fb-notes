<template>
    <div id="app">
        <Notebook @go-page="goPage" @new-page="newPage" 
        :pages="pages" :activePage="index" />
        <Page @save-page="savePage" @delete-page="deletePage" 
        :page="pages[index]" />
    </div>
</template>

<script>
 import Notebook from './components/Notebook'
 import Page from './components/Page'
 import Firebase from 'firebase'
 
 let db=Firebase.initializeApp ({
    apiKey: "AIzaSyCFeoQjfjQb4DnOzeSKkblQKFAoNmimn_w",
    authDomain: "vuecrudfbpages.firebaseapp.com",
    databaseURL: "https://vuecrudfbpages.firebaseio.com",
    projectId: "vuecrudfbpages",
    storageBucket: "vuecrudfbpages.appspot.com",
    messagingSenderId: "475096001678"
}).database ().ref ()

export default {
    name: 'app', 
    components: {
        Notebook,
        Page
    }, 
    data: ()=> ({
        pages: [],
        index: 0
    }),

    mounted() {
        database.once('value', (pages) => {
            pages.forEach((page) => {
                this.pages.push({
                    ref: page.ref,
                    title: page.child('title').val(),
                    content: page.child('content').val()
                })
            })
        })
    },
    
    methods: {
        newPage () {
            this.pages.push({
               title: '',
               content: '' 
            })
            this.index = this.pages.length - 1
        },

        goPage (nextIndex) {
            this.index = nextIndex
        },

        savePage () {
            let page = this.pages[this.index]
            if (page.ref) {
                this.updatePage(page)
            } else {
                this.insertPage(page)
            }
        },

        updatePage (page) {
            page.ref.update({
               title: page.title,
               content: page.content 
            })
        },

        insertPage (page) {
            page.ref = database.push(page)
        },

        deletePage () {
            this.pages.splice(this.index, 1)
            this.index = Math.max(this.index - 1, 0)

        }
    }
}
</script>

<style>
html, body, #app {
    height: 100%;
    }
body {
    margin: 0;
    }
#app {
    font-family: 'Avenir', Helvitica, Arial, sans-serif;
    display: flex;
    flex-direction: row; 
    }
</style>