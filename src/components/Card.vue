<template>
    <ul>
        <li v-if="thereIsCoverImg" class="cover">
            <img :src="cover" :alt="title">
        </li>
        <li class="title">Titolo: {{ title }}</li>
        <li v-if="originalTitle.toLowerCase() !== title.toLowerCase()" class="original-title">Titolo Originale: {{ originalTitle }}</li>
        <li class="lang">
            <img v-if="thereIsLangImg" :src="require(`../assets/flags/${this.lang}.png`)" :alt="lang">
            <span v-else>{{ lang }}</span>
        </li>
        <li class="rate">
            Voto: {{ rate / 2 }}
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
    </ul>
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
ul {
    .cover {
        list-style: none;
    }

    .lang {
        img {
            height: 20px;
        }
    }
}
</style>