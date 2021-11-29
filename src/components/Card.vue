<template>
    <ul class="movie">
        <li class="title">Titolo: {{ title }}</li>
        <li class="original-title">Titolo Originale: {{ originalTitle }}</li>
        <li class="lang">Lingua: <img :src="langImg" :alt="lang"></li>
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
        langImg() {
            const langAvailable = ['it', 'en'];
            if (langAvailable.includes(this.lang)) {
                return require(`../assets/flags/${this.lang}.png`);
            }
            return this.lang;
        }
    },
}
</script>

<style scoped lang="scss">
.movie {
    .lang {
        img {
            height: 20px;
        }
    }
}



</style>