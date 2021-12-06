<template>
    <div id="app" class="d-flex flex-column">
        <img
            v-if="backgroundUrl !== ''"
            class="background"
            :src="backgroundUrl" 
            alt="Background"
        >

        <Header @search="performSearch" />

        <LandingPage v-if="noResults"/>

        <main v-else class="container-fluid flex-grow-1">
            <h2 v-if="isSearchKey">
                <strong>{{ totalMoviesResults + totalTvSeriesResults }}</strong>
                risultati ({{ totalMoviesResults }} film, {{ totalTvSeriesResults }} serie tv) per '<strong>{{ searchKey }}</strong
                >'
            </h2>
            <CardList
                :list="movieList"
                titleList="Movies"
                @cardMouseover="changeBackground"
            />
            <CardList
                :list="tvSeriesList"
                titleList="Serie TV"
                @cardMouseover="changeBackground"
            />
        </main>
    </div>
</template>

<script>
import Header from "@/components/Header.vue";
import LandingPage from "@/components/LandingPage.vue";
import CardList from "@/components/CardList.vue";

import axios from "axios";

export default {
    name: "App",
    components: {
        Header,
        LandingPage,
        CardList,
    },
    data() {
        return {
            movieList: [],
            totalMoviesResults: 0,
            tvSeriesList: [],
            totalTvSeriesResults: 0,
            movieGenres: [],
            tvSeriesGenres: [],
            searchKey: '',
            backgroundUrl: '',
        };
    },
    created() {
        this.genresListRetrieve();
    },
    computed: {
        isSearchKey() {
            return this.searchKey !== '';
        },
        thereIsBackground() {
            return this.backgroundUrl !== '' || !this.backgroundUrl.includes('null');
        },
        noResults() {
            return this.movieList.length === 0 && this.tvSeriesList.length === 0;
        }
    },
    methods: {
        genresListRetrieve() {
            // Api Call with Axios for movieGenres
            axios.get('https://api.themoviedb.org/3/genre/movie/list?api_key=18eb9cfc2ae9b902fe63f5463046505f&language=it-IT')
            .then(result => this.movieGenres = result.data.genres)
            .catch(err => console.log(err));

            // Api Call with Axios for tvSeriesGenres
            axios.get('https://api.themoviedb.org/3/genre/tv/list?api_key=18eb9cfc2ae9b902fe63f5463046505f&language=it-IT')
            .then(result => this.tvSeriesGenres = result.data.genres)
            .catch(err => console.log(err));
        },
        performSearch(text) {
            if (text !== "") {
                // Array reset
                this.movieList = [];
                this.tvSeriesList = [];

                // Save search key
                this.searchKey = text;

                // Api Params
                const apiParams = {
                    api_key: "18eb9cfc2ae9b902fe63f5463046505f",
                    language: "it-IT",
                    query: text,
                };

                // Api Call with Axios for movieList
                axios
                    .get("https://api.themoviedb.org/3/search/movie/", {
                        params: apiParams,
                    })
                    .then((result) => {
                        this.totalMoviesResults = result.data.total_results;
                        result.data.results.forEach(e => {
                            this.movieList.push({
                                id: e.id,
                                genres: e.genre_ids.map(genre => {
                                    for (let i = 0; i < this.movieGenres.length; i++) {
                                        if (genre === this.movieGenres[i].id) {
                                            return this.movieGenres[i].name;
                                        }
                                    }
                                }),
                                title: e.title,
                                originalTitle: e.original_title,
                                lang: e.original_language,
                                rate: e.vote_average,
                                description: e.overview,
                                background: e.backdrop_path,
                                cover: e.poster_path
                            });
                        });
                    })
                    .catch((err) => console.log(err));

                // Api Call with Axios for tvSeriesList
                axios
                    .get("https://api.themoviedb.org/3/search/tv/", {
                        params: apiParams,
                    })
                    .then((result) => {
                        this.totalTvSeriesResults = result.data.total_results;
                        result.data.results.forEach(e => {
                            this.tvSeriesList.push({
                                id: e.id,
                                genres: e.genre_ids.map(genre => {
                                    for (let i = 0; i < this.tvSeriesGenres.length; i++) {
                                        if (genre === this.tvSeriesGenres[i].id) {
                                            return this.tvSeriesGenres[i].name;
                                        }
                                    }
                                }),
                                title: e.name,
                                originalTitle: e.original_name,
                                lang: e.original_language,
                                rate: e.vote_average,
                                description: e.overview,
                                background: e.backdrop_path,
                                cover: e.poster_path
                            });
                        });
                    })
                    .catch((err) => console.log(err));
            }
        },
        changeBackground(endpoint) {
            this.backgroundUrl = `https://image.tmdb.org/t/p/original${endpoint}`;
        }
    },
};
</script>

<style lang="scss">
// GENERAL
img {
    display: block;
}

// APP
#app {
    position: relative;
    min-height: 100vh;
    background: #333;
    overflow-y: hidden;

    img.background {
        position: fixed;
        width: 100%;
        height: 100vh;
        object-fit: cover;
    }

    main {
        padding: calc(3rem + 126px) 3rem 3rem;
        background: rgba(33,33,33,0.5);
        color: #fff;
        overflow: auto;
        z-index: 1;

        h2 {
            font-weight: 400;
        }
    }
}
</style>
