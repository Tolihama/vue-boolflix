<template>
    <div id="app">
        <Header @search="apiCall" />

        <main>
            <Card 
                v-for="(item, index) in apiArray"
                :key="`movie-${index}`"
                :title="item.title"
                :originalTitle="item.original_title"
                :lang="item.original_language"
                :rate="item.vote_average"
            />
        </main>
    </div>
</template>

<script>
import Header from '@/components/Header.vue';
import Card from '@/components/Card.vue';

import axios from 'axios';

export default {
    name: "App",
    components: {
        Header,
        Card
    },
    data() {
        return {
            apiArray: [],
        }
    },
    methods: {
        apiCall(text) {
            if (text !== '') {
                // Array reset
                this.apiArray = [],

                // Api Call with Axios
                axios.get('https://api.themoviedb.org/3/search/movie/', {
                    params: {
                        api_key: '18eb9cfc2ae9b902fe63f5463046505f',
                        query: text,
                    },
                })
                .then(result => result.data.results.forEach(e => this.apiArray.push(e)))
                .catch(err => console.log(err));
            }
        },
    },
};
</script>

<style lang="scss">

</style>
