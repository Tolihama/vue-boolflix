<template>
    <ul>
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
    },
}
</script>

<style scoped lang="scss">
ul {
    .lang {
        img {
            height: 20px;
        }
    }
}
</style>