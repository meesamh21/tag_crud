<template>
    <div>
        <h1>Tags</h1>

        <h2>List of Tags</h2>
        <ul>
            <li v-for="tag in tags" :key="tag.id" style="list-style: none;">
                Name: {{ tag.name }} - Start time: {{ tag.start_time }} - End time: {{ tag.end_time }}
                <button @click="deleteTag(tag.id)">X</button>
            </li>
        </ul>

        <h2>Create New Tag</h2>
        <form>
            <label for="name">Name</label>
            <input id="name" v-model="newTag.name" placeholder="Tag Name">

            <label for="start-time">Start Time</label>
            <input id="start-time" v-model="newTag.startTime" placeholder="Start time">

            <label for="end-time">End Time</label>
            <input id="end-time" v-model="newTag.endTime" placeholder="End Time">

            <button type="submit" @click="saveTag">Save</button>
        </form>

        <h2>Search Tag</h2>
        <label for="search-name">Search</label>
        <input id="search-name" v-model="searchName" placeholder="Search Name">
        <button @click="findTag(searchName)">Search</button>
        <div v-if="searchResult.name">
            Name: {{ searchResult.name }} - Start time: {{ searchResult.start_time }} - End time: {{ searchResult.end_time }}
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data: function () {
    return {
        tags: [],
        searchName: "",
        searchResult: {},
        newTag: {
            name: "",
            startTime: "",
            endTime: ""
        }
    }
    },
    methods: {
        saveTag(e) {
            e.preventDefault()
            axios.post("http://localhost:3000/tag", {
                name: this.newTag.name,
                start_time: this.newTag.startTime,
                end_time: this.newTag.endTime
            })
            .then(() => {
                this.getTags()
                this.newTag = {
                    name: "",
                    startTime: "",
                    endTime: ""
                }
            })
        },
        getTags() {
            axios.get("http://localhost:3000/tags")
            .then(response => {
                this.tags = response.data
            })
        },
        findTag(name) {
            axios.get(`http://localhost:3000/tag?name=${name}`)
            .then(response => {
                this.searchResult = response.data
            })
        },
        deleteTag(id) {
            axios.delete(`http://localhost:3000/tag/${id}`)
            .then(response => {
                console.log(response)
                this.getTags()
            })
        }
    },
    created() {
        this.getTags()
    }
}
</script>