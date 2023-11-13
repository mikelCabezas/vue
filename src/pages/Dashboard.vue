<template>
    <div id="app" class="ease-linear duration-200" :style="headerMainSpacing">
        <Header :users="users" :userId="userId" @handle-add-to-grid="handleAddToGrid" @handle-hover-header="handleHoverHeader" @unhandle-hover-header="unHandleHoverHeader" />
        <Grid :user="userId" :card="component" />
    </div>
</template>

<script>
import { defineComponent } from 'vue'
import Grid from '../components/Grid.vue'
import Header from '../components/Header.vue'

export default defineComponent({
    components: {
        Grid,
        Header
    },
    data() {
        return {
            userId: this.$route.params.userId,
            currentUser: null,
            users: [],
            component: null,
            userLocalComponents: [],
            headerMainSpacing: 'padding-left: 5rem;'
        }
    },
    async created() {
        const response = await fetch("https://run.mocky.io/v3/c2c1f79c-3403-4406-be1b-cf9220d23c28");
        const data = await response.json();
        data.data.forEach(data => {
            delete data.data
        })
        this.users = data.data;
        this.getUser(this.userId)
        this.getLocalStorage()
    },

    methods: {
        handleAddToGrid: function (value) {
            this.component = value
        },
        handleHoverHeader: function (value) {
            this.headerMainSpacing = 'padding-left: 18rem;'
        },
        unHandleHoverHeader: function (value) {
            this.headerMainSpacing = 'padding-left: 5rem;'
        },
        getUser: function (userId) {
            userId = +userId
            this.currentUser = this.users.find((user) => user.id === userId)
            localStorage.setItem('currentUser', JSON.stringify(this.currentUser))
        },
        getLocalStorage: function () {
            const getLocalComponents = JSON.parse(localStorage.getItem('currentComponents'));

            if (!getLocalComponents) {
                const currentComponents = []
                this.users.forEach(_user => {
                    const user = {
                        id: _user.id,
                        name: _user.name,
                        email: _user.email,
                        components: []
                    }
                    currentComponents.push(user)
                })
                localStorage.setItem('currentComponents', JSON.stringify(currentComponents))
            }
            if (getLocalComponents)
                this.userLocalComponents = getLocalComponents.find(user => user.id === +this.userId)
        }
    },
})
</script>