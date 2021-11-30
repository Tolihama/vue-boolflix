<template>
    <div>
        <h3 v-if="isOutput">{{ titleList }}</h3>
        <Card 
            v-for="item in list"
            :key="item.id"
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
            return this.list.length === 0 ? false : true;
        }
    },
    props: {
        list: Array,
        titleList: String,
        apiPropTitle: String,
        apiPropOriginalTitle: String,
        apiPropLang: String,
        apiPropRate: String,
    }
}
</script>

<style scoped lang="scss">

</style>