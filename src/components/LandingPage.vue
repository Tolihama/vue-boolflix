<template>
    <div class="landing flex-grow-1">
        <div class="landing-text d-flex flex-column justify-content-center text-center">
            <i class="fas fa-tv icon"></i>
            <h2>Inizia la tua ricerca!</h2>
        </div>
    </div>

</template>

<script>
export default {
    name: 'LandingPage',
}
</script>

<style scoped lang="scss">
.landing {
    position: relative;
    height: 100%;

    img {
        position: absolute;
        width: 100%;
        height: 100vh;
        object-fit: cover;
        z-index: 0;
    }

    .landing-text {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        animation: pulse 2s ease-in-out infinite alternate;
        color: #fff;
        z-index: 1;

        .icon {
            font-size: 4rem;
            padding: 2rem;
        }

        h2 {
            font-size: 3rem;
            font-weight: 700;
        }
    }
}

@keyframes pulse {
    to {
        transform: translate(-50%, -50%) scale(130%);
    }
}

</style>