<template>
    <header>
        <input 
            v-model.trim="inputSearch"
            type="text"
            @keyup.enter="$emit('search', inputSearch)"
            @keyup="clear"
        >
        <button 
            @click="$emit('search', inputSearch)"
        >
            Search
        </button>
    </header>
</template>

<script>
export default {
    name: 'Header',
    data() {
        return {
            inputSearch: '',
        }
    },
    methods: {
        clear(input) {
            if (input.key === 'Delete') {
                this.inputSearch = '';
            }
        },
    },
}
</script>

<style scoped lang="scss">

</style>