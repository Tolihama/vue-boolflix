<template>
    <div>
        <h3 v-if="isOutput" class="d-flex align-items-center h1 pt-5 pb-2 fw-bold">
            {{ titleList }}
        </h3>
        <div class="row pb-4 card-list">
            <Card 
                v-for="(item, i) in list"
                :key="i"
                :item="item"
                @cardMouseover="cardMouseoverTrigger"
                @cardClicked="cardClickedTrigger"
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
    },
    methods: {
        cardMouseoverTrigger(id) {
            console.log(id);
            this.$emit('cardMouseover', id);
        },
        cardClickedTrigger(activeItem) {
            this.$emit('cardClicked', activeItem);
        }
    }
}
</script>

<style scoped lang="scss">
h3 {
    text-transform: uppercase;
    letter-spacing: .3rem;

    &::before {
        content: url(../assets/B_logo.png);
        margin-right: 2rem;
    }
}

/* .card-list {
    flex-wrap: nowrap;
    overflow-x: auto;
} */
</style>