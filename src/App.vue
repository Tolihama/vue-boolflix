<template>
    <div id="app" class="d-flex flex-column">
        <Header @search="performSearch" />

        <img 
            class="background active"
            src="./assets/landing_background.jpg" 
            alt="Boolfix Default Background"
        >
        <img
            v-for="bg in backgroundList"
            class="background"
            :key="`bg-${bg.type}-${bg.id}`"
            :src="bg.url" 
            :alt="`Background ${bg.type}-${bg.id}`"
            :class="{active : `${bg.type}-${bg.id}` === mouseOnItemId}"
        >

        <transition name="fade" mode="out-in">
            <LandingPage v-if="noResults"/>
            <main v-else class="container-fluid flex-grow-1">
                <transition name="fade" mode="out-in">
                    <div v-if="!thereIsActiveItem">
                        <h2 v-if="isSearchKey">
                            <strong>{{ totalMoviesResults + totalTvSeriesResults }}</strong>
                            risultati ({{ totalMoviesResults }} film, {{ totalTvSeriesResults }} serie tv) per '<strong>{{ searchKey }}</strong
                            >'
                        </h2>
                        <CardList
                            :list="movieList"
                            titleList="Movie"
                            @cardMouseover="changeOnMouseId"
                            @cardClicked="openCardDetails"
                        />
                        <CardList
                            :list="tvSeriesList"
                            titleList="Serie TV"
                            @cardMouseover="changeOnMouseId"
                            @cardClicked="openCardDetails"
                        />
                    </div>
                    <CardDetails 
                        v-else
                        :infos="activeItem" 
                        @cardDetailsClicked="closeCardDetails"
                    />
                </transition>
            </main>
        </transition>
    </div>
</template>

<script>
import Header from "@/components/Header.vue";
import LandingPage from "@/components/LandingPage.vue";
import CardDetails from "@/components/CardDetails.vue";
import CardList from "@/components/CardList.vue";

import axios from "axios";

export default {
    name: "App",
    components: {
        Header,
        LandingPage,
        CardDetails,
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
            mouseOnItemId: null,
            activeItem: null,
        };
    },
    created() {
        this.genresListRetrieve();
    },
    computed: {
        isSearchKey() {
            return this.searchKey !== '';
        },
        backgroundList() {
            const backgroundList = [];
            const movieAndTvList = [...this.movieList, ...this.tvSeriesList];
            movieAndTvList.forEach(item => {
                if (item.background !== null) {
                    backgroundList.push({
                        url: 'https://image.tmdb.org/t/p/original' + item.background,
                        type: item.type,
                        id: item.id
                    })
                }
            });
            return backgroundList;
        },
        noResults() {
            return this.movieList.length === 0 && this.tvSeriesList.length === 0;
        },
        thereIsActiveItem() {
            return this.activeItem !== null;
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
                                cover: e.poster_path,
                                type: 'movie'
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
                                cover: e.poster_path,
                                type: 'tv'
                            });
                        });
                    })
                    .catch((err) => console.log(err));
            }
        },
        changeOnMouseId(id) {
            this.mouseOnItemId = id;
        },
        openCardDetails(activeItem) {
            this.activeItem = activeItem;
        },
        closeCardDetails() {
            this.activeItem = null;
        }
    },
};
</script>

<style lang="scss">
// GENERAL
img {
    display: block;
}

// TRANSITION
.fade-enter-active, .fade-leave-active {
    transition: opacity .5s ease;
}

.fade-enter, .fade-leave-to {
    opacity: 0;
}

// APP
#app {
    position: relative;
    height: 100vh;
    background: #333;
    overflow-y: hidden;

    img.background {
        position: fixed;
        width: 100%;
        height: 100vh;
        object-fit: cover;
        opacity: 0;
        transition: opacity 1s;

        &.active {
            opacity: 1;
        }
    }

    main {
        padding: calc(3rem + 126px) 3rem 3rem;
        background: rgba(33,33,33,0.5);
        color: #fff;
        overflow-y: auto;
        z-index: 1;

        h2 {
            font-weight: 400;
        }
    }
}
</style>