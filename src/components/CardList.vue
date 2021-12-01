<template>
    <div>
        <h3 v-if="isOutput" class="text-center py-3 fw-bold h2">{{ titleList }}</h3>
        <div class="row justify-content-center">
            <Card 
                v-for="item in list"
                :key="item.id"
                :cover="coverBaseUrl + item[apiPropCover]"
                :title="item[apiPropTitle]"
                :originalTitle="item[apiPropOriginalTitle]"
                :lang="item[apiPropLang]"
                :rate="item[apiPropRate]"
                :description="item[apiPropDesc]"
            />
        </div>
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
        apiPropDesc: String,
    },
}
</script>

<style scoped lang="scss">

</style>