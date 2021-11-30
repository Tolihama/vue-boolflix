<template>
    <ul>
        <li v-if="thereIsCoverImg" class="cover">
            <img :src="cover" :alt="title">
        </li>
        <li class="title">Titolo: {{ title }}</li>
        <li class="original-title">Titolo Originale: {{ originalTitle }}</li>
        <li class="lang">
            <img v-if="thereIsLangImg" :src="require(`../assets/flags/${this.lang}.png`)" :alt="lang">
            <span v-else>{{ lang }}</span>
        </li>
        <li class="rate">Voto: {{ rate }}</li>
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