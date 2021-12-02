<template>
    <div id="app" class="d-flex flex-column">
        <img
            class="background"
            :src="backgroundUrl" 
            alt="Background"
        >

        <Header @search="performSearch" />

        <main class="container-fluid flex-grow-1">
            <h2 v-if="isSearchKey">
                <strong>{{ totalMoviesResults + totalTvSeriesResults }}</strong>
                risultati ({{ totalMoviesResults }} film, {{ totalTvSeriesResults }} serie tv) per '<strong>{{ searchKey }}</strong
                >'
            </h2>
            <CardList
                :list="movieList"
                titleList="Movies"
                coverBaseUrl="https://image.tmdb.org/t/p/w342/"
                apiPropCover="poster_path"
                apiPropBackground="backdrop_path"
                apiPropTitle="title"
                apiPropOriginalTitle="original_title"
                apiPropLang="original_language"
                apiPropRate="vote_average"
                apiPropDesc="overview"
                @cardHover="changeBackground"
            />
            <CardList
                :list="tvSeriesList"
                titleList="Serie TV"
                coverBaseUrl="https://image.tmdb.org/t/p/w342/"
                apiPropCover="poster_path"
                apiPropBackground="backdrop_path"
                apiPropTitle="name"
                apiPropOriginalTitle="original_name"
                apiPropLang="original_language"
                apiPropRate="vote_average"
                apiPropDesc="overview"
                @cardHover="changeBackground"
            />
        </main>
    </div>
</template>

<script>
import Header from "@/components/Header.vue";
import CardList from "@/components/CardList.vue";

import axios from "axios";

export default {
    name: "App",
    components: {
        Header,
        CardList,
    },
    data() {
        return {
            movieList: [],
            totalMoviesResults: 0,
            tvSeriesList: [],
            totalTvSeriesResults: 0,
            searchKey: '',
            backgroundUrl: '',
        };
    },
    computed: {
        isSearchKey() {
            return this.searchKey !== "";
        },
        thereIsBackground() {
            return this.backgroundUrl !== '' || !this.backgroundUrl.includes('null');
        }
    },
    methods: {
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
                        result.data.results.forEach((e) =>
                            this.movieList.push(e)
                        );
                    })
                    .catch((err) => console.log(err));

                // Api Call with Axios for tvSeriesList
                axios
                    .get("https://api.themoviedb.org/3/search/tv/", {
                        params: apiParams,
                    })
                    .then((result) => {
                        this.totalTvSeriesResults = result.data.total_results;
                        result.data.results.forEach((e) =>
                            this.tvSeriesList.push(e)
                        );
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
