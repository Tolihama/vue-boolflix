<template>
    <div>
        <h3 v-if="isOutput">{{ titleList }}</h3>
        <Card 
            v-for="item in list"
            :key="item.id"
            :cover="coverBaseUrl + item[apiPropCover]"
            :title="item[apiPropTitle]"
            :originalTitle="item[apiPropOriginalTitle]"
            :lang="item[apiPropLang]"
            :rate="item[apiPropRate]"
        />
    </div>
</template>

<script>
import Card from '@/components/Card.vue';

export default {
    name: 'CardList',
    components: {
        Card,
    },
    computed: {
        isOutput() {
            return this.list.length !== 0;
        },
    },
    props: {
        list: Array,
        titleList: String,
        coverBaseUrl: String,
        apiPropCover: String,
        apiPropTitle: String,
        apiPropOriginalTitle: String,
        apiPropLang: String,
        apiPropRate: String,
    },
}
</script>

<style scoped lang="scss">

</style>