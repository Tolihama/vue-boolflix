<template>
    <header class="d-flex align-items-center px-5">
        <div class="logo flex-grow-1">
            <img src="@/assets/boolfix.png" alt="Boolfix Logo">
        </div>

        <div class="input-search d-flex aic">
            <input
                class="flex-shrink-0"
                v-model.trim="inputSearch"
                type="text"
                placeholder="Fai la tua ricerca"
                @keyup.enter="$emit('search', inputSearch)"
                @keyup="clear"
            >
        </div>
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
    background: linear-gradient(
        to bottom, 
        rgba(0,0,0,.8) 0%, 
        rgba(0,0,0,.2) 90%,
        rgba(0,0,0,0) 100%
    );
    padding: 20px;
    top: 0;
    position: fixed;
    width: 100%;
    z-index: 10;

    .input-search {
        margin-right: 2rem;

        &::before {
            content: '\f002';
            font-family: 'Font Awesome 5 Free';
            font-weight: 700;
            font-size: 1.7rem;
            margin-right: 2rem;
            color: #fff;
        }

        input {
            background: transparent;
            border: 0;
            border-bottom: 1px solid #555;
            color: #fff;
            font-size: 1.2rem;
            width: 15%;
            min-width: 300px;
            padding: 0 1.5rem;

            &:focus-visible {
                border-bottom: 1px solid #aaa;
                outline-style: none;
                background: linear-gradient(
                    to top,
                    rgba(255,255,255,.1) 0%,
                    rgba(255,255,255,0) 100%
                );
            }
            
        }
    }
}
</style>