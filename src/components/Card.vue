<template>
    <div 
        class="col-sm-12 col-md-6 col-lg-4 col-xl-2 col px-1 py-3"
        @mouseover="$emit('cardMouseover', `${item.type}-${item.id}`)"
        @click="cardClick"
    >
        <div class="card">
            <div class="cover">
                <img v-if="thereIsCoverImg" :src="`https://image.tmdb.org/t/p/w342${item.cover}`" :alt="item.title" />
                <img
                    v-else
                    class="w-100"
                    src="@/assets/image-not-found.png"
                    :alt="item.title"
                />
            </div>
            <ul class="text d-flex flex-column justify-content-center">
                <li class="title text-center">
                    <h3 class="fw-bold m-0">{{ item.title }}</h3>
                </li>
                <li
                    v-if="item.originalTitle.toLowerCase() !== item.title.toLowerCase()"
                    class="original-title text-center"
                >
                    <h4>({{ item.originalTitle }})</h4>
                </li>
                <li class="lang d-flex justify-content-center">
                    <img
                        v-if="thereIsLangImg"
                        :src="require(`../assets/flags/${item.lang}.png`)"
                        :alt="item.lang"
                    />
                    <span v-else>{{ item.lang }}</span>
                </li>
                <li class="rate text-center py-4">
                    <div class="pb-2 fs-5">Voto: {{ item.rate / 2 }}</div>
                    <i
                        v-for="(n, i) in stars"
                        :key="`rate-${i}`"
                        class="fas fa-star icon"
                    ></i>
                    <i
                        v-for="(n, i) in 5 - stars"
                        :key="`empty-${i}`"
                        class="far fa-star icon"
                    ></i>
                </li>
                <li
                    v-if="item.genres.length > 0"
                    class="genres py-3 text-center"
                >
                    <strong>Generi:</strong><br>
                    <span 
                        v-for="genre in genresList"
                        :key="`${item.id}-${genre}`"
                    >
                        {{ genre }}
                    </span>
                </li>
                <li class="show-infos text-center fs-5 fw-bold d-flex flex-column justify-content-end flex-grow-1">
                    Scopri di più!<br>
                    <i class="fas fa-info-circle"></i>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
import axios from "axios";

export default {
    name: "Card",
    data() {
        return {
            cast: [],
            activeItem: null,
        }
    },
    props: {
        item: Object,
    },
    created() {
        this.castRetrieve();
    },
    computed: {
        thereIsLangImg() {
            const langAvailable = ["it", "en"];
            return langAvailable.includes(this.item.lang);
        },
        thereIsCoverImg() {
            return this.item.cover !== null;
        },
        stars() {
            return Math.ceil(this.item.rate / 2);
        },
        genresList() {
            const genresList = [...this.item.genres];
            for (let i = 0; i < genresList.length - 1; i++) {
                genresList[i] += ', ';
            }
            return genresList;
        }
    },
    methods: {
        cardClick() {
            this.$emit('cardClicked', this.activeItem);
        },
        castRetrieve() {
            axios.get(`https://api.themoviedb.org/3/${this.item.type}/${this.item.id}/credits?api_key=18eb9cfc2ae9b902fe63f5463046505f&language=it-IT`)
            .then(result => {
                this.cast = result.data.cast;
                this.activeItem = {...this.item, cast: result.data.cast};
            })
            .catch(err => console.log(err));
        }
    },
};
</script>

<style scoped lang="scss">
.card {
    height: 500px;
    position: relative;
    overflow: hidden;
    box-shadow: 0 5px 10px rgba(0, 0, 0, 0.5), 0 0 5px 1px #000;
    border: 0;
    border-radius: 30px;
    transition: transform .5s;

    &:hover {
        transform: scale(110%);
        z-index: 1;
    }

    .cover img {
        position: absolute;
        width: 100%;
        height: 100%;
        object-fit: cover;
    }

    ul.text {
        list-style: none;
        position: relative;
        opacity: 0;
        cursor: pointer;
        transition: all 1s;
        background: rgba(0, 0, 0, 0.8);
        padding: 1rem;
        margin: 0;
        width: 100%;
        height: 100%;
        overflow-y: auto;
        border: 1px solid #000;
        box-shadow: 0 0 50px 1px rgba(255, 255, 255, 0.5) inset;
        z-index: 1;

        &:hover {
            opacity: 1;
            transition: opacity 1s;
        }

        li {
            padding: 5px 0;
        }

        .lang {
            img {
                height: 20px;
            }
        }

        .rate {         
            .icon {
                color: yellow;
                font-size: 1.5rem;
            }
        }
    }
}
</style>