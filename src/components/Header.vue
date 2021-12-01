<template>
    <header class="d-flex align-items-center">
        <div class="logo flex-grow-1">
            <img src="@/assets/boolfix.png" alt="Boolfix Logo">
        </div>

        <input 
            v-model.trim="inputSearch"
            type="text"
            placeholder="Fai la tua ricerca"
            @keyup.enter="$emit('search', inputSearch)"
            @keyup="clear"
        >
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
header {
    background: #111;
    padding: 20px;

    input {
        width: 15%;
        min-width: 300px;
        padding: .5rem 1.5rem;
        border-radius: 100rem;
    }
}
</style>