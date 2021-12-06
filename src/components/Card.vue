<template>
    <div 
        class="col-sm-12 col-md-6 col-lg-4 col-xl-2 col px-1 py-3"
        @mouseover="$emit('cardMouseover', item.background)"
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
                <li class="rate text-center pt-3 pb-5">
                    Voto: {{ item.rate / 2 }}<br />
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
                    class="genres"
                >
                    Generi:
                    <span 
                        v-for="genre in item.genres"
                        :key="`${item.id}-${genre}`"
                    >
                        {{ genre }}
                    </span>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
    name: "Card",
    props: {
        item: Object,
    },
    computed: {
        thereIsLangImg() {
            const langAvailable = ["it", "en"];
            return langAvailable.includes(this.item.lang);
        },
        thereIsCoverImg() {
            return !this.item.cover.includes("null");
        },
        stars() {
            return Math.ceil(this.item.rate / 2);
        },
    },
};
</script>

<style scoped lang="scss">
.card {
    height: 500px;
    position: relative;
    overflow: hidden;
    // box-shadow: 0 5px 10px rgba(0, 0, 0, 0.5);
    border: 0;

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
        transition: opacity 0.5s;
        background: rgba(0, 0, 0, 0.8);
        padding: 1rem;
        margin: 0;
        width: 100%;
        height: 100%;
        overflow-y: auto;
        box-shadow: 0 0 20px rgba(0, 0, 0, 0.5) inset;
        z-index: 1;

        &:hover {
            opacity: 1;
        }

        li {
            padding: 5px 0;
        }

        .lang {
            img {
                height: 20px;
            }
        }

        .rate .icon {
            color: yellow;
            font-size: 2rem;
        }

        .genres {
            span::after {
                content: ', ';
            }

            span:last-child::after {
                content: '';
            }
        }
    }
}
</style>