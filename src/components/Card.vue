<template>
    <ul>
        <li>Titolo: {{ title }}</li>
        <li>Titolo Originale: {{ originalTitle }}</li>
        <li>Lingua: {{ lang }}</li>
        <li>Voto: {{ rate }}</li>
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
}
</script>

<style scoped lang="scss">

</style>