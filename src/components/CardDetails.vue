<template>
    <div class="container d-flex flex-column px-5 pt-5">
        <div class="top d-flex pb-5">
            <div class="titles flex-grow-1 pe-3">
                <h2 class="fw-bold h1">{{ infos.title }}</h2>
                <h3 v-if="!titleCoincidence">{{ infos.originalTitle }}</h3>
            </div>
            <div class="close-icon h1" @click="$emit('cardDetailsClicked')">
                <i class="far fa-times-circle"></i>
            </div>
        </div>

        <div class="bottom d-flex pb-5">
            <div class="cover flex-shrink-0">
                <img :src="`https://image.tmdb.org/t/p/w342${infos.cover}`" :alt="infos.title">
            </div>
            <ul class="text fs-4 flex-grow-1">
                <li class="rate py-1">
                    <strong>Voto:</strong> {{ infos.rate / 2 }} <br>
                    <i
                        v-for="(n, i) in stars"
                        :key="`rate-${i}`"
                        class="fas fa-star icon"
                    ></i>
                    <i
                        v-for="(n, i) in 5 - stars"
                        :key="`empty-${i}`"
                        class="far fa-star icon"
                    ></i>
                </li>
                <li class="genres py-1">
                    <strong>Generi:</strong>
                    <span 
                        v-for="genre in infos.genres" 
                        :key="`${infos.id}-${genre}`"
                        class="genre"
                    >
                        {{ genre }}
                    </span>
                </li>
                <li class="cast">
                    <strong>Cast:</strong><br>
                    <span
                        v-for="actor in actorList"
                        :key="`${infos.id}-${actor.id}`"
                        class="actor"
                    >
                        {{ actor.name }}
                    </span><br>
                    <span 
                        v-if="isActorListReduced" 
                        @click="toggleActorList"
                        class="link"
                    >
                        [ <i class="fas fa-plus"></i> Espandi]
                    </span>
                    <span 
                        v-else 
                        @click="toggleActorList"
                        class="link"
                    >
                        [ <i class="fas fa-minus"></i> Riduci]
                    </span>
                </li>
                <li class="description pt-4">
                    <strong>Descrizione:</strong><br>
                    {{ infos.description }}
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
    name: 'CardDetails',
    props: {
        infos: Object,
    },
    data() {
        return {
            isActorListReduced: true,
        }
    },
    computed: {
        titleCoincidence() {
            return this.infos.title.toLowerCase() === this.infos.originalTitle.toLowerCase();
        },
        stars() {
            return Math.ceil(this.infos.rate / 2);
        },
        completeActorList() {
            const actorList = this.infos.cast.map(actor => {
                return {name: actor.name, id: actor.id};
            });
            for (let i = 0; i < actorList.length - 1; i++) {
                actorList[i].name += ', ';
            }
            return actorList;
        },
        reducedActorList() {
            const reducedActorList = this.infos.cast.slice(0, 5).map(actor => {
                return {name: actor.name, id: actor.id};
            });
            for (let i = 0; i < reducedActorList.length - 1; i++) {
                reducedActorList[i].name += ', ';
            }
            return reducedActorList;
        },
        actorList() {
            return this.isActorListReduced ? this.reducedActorList : this.completeActorList;
        }
    },
    methods: {
        toggleActorList() {
            this.isActorListReduced = !this.isActorListReduced;
        }
    }
}
</script>

<style scoped lang="scss">
.container {
    background: rgba(0,0,0,.5);
    border-radius: 30px;
    max-height: 100%;
    overflow: hidden;
}

.close-icon {
    cursor: pointer;
    transition: all .3s;

    &:hover {
        color: rgb(255,50,0);
    }
}

.bottom {
    overflow: auto;
    .cover {
        max-width: 25%;

        img {
            width: 100%;
        }
    }

    ul.text {
        list-style: none;
        overflow-y: auto;

        .rate {
            .icon {
                color: orange;
            }
        }
    }
}

.link {
    color: rgb(0,150,250);
    font-weight: 600;
    cursor: pointer;

    &:hover {
        color: rgb(100, 220, 250);
    }

    i {
        font-size: 20px;
    }


}

</style>