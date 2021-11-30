<template>
    <div id="app">
        <Header @search="performSearch" />

        <main>
            <h2 v-if="isSearchKey">
                In archivio ci sono <strong>{{ movieList.length + tvSeriesList.length }}</strong> risultati per '<strong>{{ searchKey }}</strong>'
            </h2>
            <div class="card-list-container">
                <CardList 
                    :list="movieList" 
                    titleList="Movies"
                    coverBaseUrl="https://image.tmdb.org/t/p/w342/"
                    apiPropCover="poster_path"
                    apiPropTitle="title"
                    apiPropOriginalTitle="original_title"
                    apiPropLang="original_language"
                    apiPropRate="vote_average"
                />
                <CardList 
                    :list="tvSeriesList" 
                    titleList="Serie TV"
                    coverBaseUrl="https://image.tmdb.org/t/p/w342/"
                    apiPropCover="poster_path"
                    apiPropTitle="name"
                    apiPropOriginalTitle="original_name"
                    apiPropLang="original_language"
                    apiPropRate="vote_average"
                />
            </div>
        </main>
    </div>
</template>

<script>
import Header from '@/components/Header.vue';
import CardList from '@/components/CardList.vue';

import axios from 'axios';

export default {
    name: "App",
    components: {
        Header,
        CardList
    },
    data() {
        return {
            movieList: [],
            tvSeriesList: [],
            searchKey: '',
        }
    },
    computed: {
        isSearchKey() {
            return this.searchKey !== '';
        }
    },
    methods: {
        performSearch(text) {
            if (text !== '') {
                // Array reset
                this.movieList = [];
                this.tvSeriesList = [];

                // Save search key
                this.searchKey = text;

                // Api Call with Axios for movieList
                this.apiCall('https://api.themoviedb.org/3/search/movie/', text, this.movieList);

                // Api Call with Axios for tvSeriesList
                this.apiCall('https://api.themoviedb.org/3/search/tv/', text, this.tvSeriesList);
            }
        },
        apiCall(endpoint, queryKey, destinationArray) {
            axios.get(endpoint, {
                params: {
                    api_key: '18eb9cfc2ae9b902fe63f5463046505f',
                    language: 'it-IT',
                    query: queryKey,
                }
            })
            .then(result => result.data.results.forEach(e => destinationArray.push(e)))
            .catch(err => console.log(err));
        }
    },
};
</script>

<style lang="scss">
// GENERAL
* {
    // margin: 0;
    // padding: 0;
    // box-sizing: border-box;
    font-family: sans-serif;
}

main {
    h2 {
        font-weight: 400;
    }
}

.card-list-container {
    display: flex;
    width: 100%;

    & > * {
        flex-grow: 1;
    }
}
</style>
