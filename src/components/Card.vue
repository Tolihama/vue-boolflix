<template>
    <div class="col-sm-12 col-md-6 col-lg-4 col-xl-2 col p-3">
        <div class="card">
            <div class="cover">
                <img v-if="thereIsCoverImg" class="w-100" :src="cover" :alt="title">
                <img v-else class="w-100" src="@/assets/image-not-found.png" :alt="title">
            </div>
            <ul>
                <li class="title text-center">
                    <h3 class="fw-bold m-0">{{ title }}</h3>
                </li>
                <li 
                    v-if="originalTitle.toLowerCase() !== title.toLowerCase()" 
                    class="original-title text-center"
                >
                    <h4>({{ originalTitle }})</h4>
                </li>
                <li class="lang d-flex justify-content-center">
                    <img v-if="thereIsLangImg" :src="require(`../assets/flags/${this.lang}.png`)" :alt="lang">
                    <span v-else>{{ lang }}</span>
                </li>
                <li class="rate text-center">
                    Voto: {{ rate / 2 }}<br> 
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
                <li class="description">{{ description }}</li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Card',
    props: {
        cover: String,
        title: String,
        originalTitle: String,
        lang: String,
        rate: Number,
        description: String,
    },
    computed: {
        thereIsLangImg() {
            const langAvailable = ['it', 'en'];
            return langAvailable.includes(this.lang);
        },
        thereIsCoverImg() {
            return !this.cover.includes('null');
        },
        stars() {
            return Math.ceil(this.rate / 2);
        }
    },
}
</script>

<style scoped lang="scss">
.card {
    display: block;
    border-radius: 30px;
    position: relative;
    overflow: hidden;
    box-shadow: 0 5px 10px rgba(0,0,0,.5);

    ul {
        position: absolute;
        opacity: 0;
        cursor: pointer;
        transition: opacity .5s;
        background: rgba(0,0,0,.8);
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        padding: 1rem;
        margin: 0;
        width: 100%;
        height: calc(100% + 1px);
        max-height: calc(100% + 1px);
        box-shadow: 0 0 20px rgba(0,0,0,.5) inset;

        &:hover {
            opacity: 1;
        }
    }
}

ul {
    list-style: none;

    li {
        padding: 5px 0;
    }

    .lang {
        img {
            height: 20px;
        }
    }
}
</style>