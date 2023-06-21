<template>
    <Loader v-if="loading" />

    <div v-else>
        <AppHeader />
        <div class="container opacity-effect">
            <div class="row py-5">

                <div class="col-12 col-md-6 p-3">
                    <div class="bg-content">

                        <div>
                            <h5>Benvenuto in</h5>

                            <h1 class="ps-3">Laravel Boolfolio</h1>

                        </div>

                        <div class="d-flex justify-content-center pb-4">
                            <img src="../assets/img/book.gif" alt="book" class="book">
                        </div>

                        <i>Lorem ipsum dolor sit amet consectetur adipisicing elit. Totam, harum! Dolores repellendus animi pariatur optio labore natus nam quas! Nemo, ipsam rem! Labore nisi rem reprehenderit dicta eos quisquam odit.</i>

                    </div>

                </div>

                <div class="col-12 col-md-6 p-3">

                    <div class="bg-content">

                        <div class="al-carousel">

                            <div v-if="dim > 1" @click.stop="goUp()" class="al-rounded up">
                                <i class="fa-solid fa-chevron-up"></i>
                            </div>



                            <div v-if="dim > 0" class="h-100 d-flex flex-column justify-content-center align-items-center gap-4 text-center">
                                <h5>{{ projects[activeSlide].name }}</h5>

                                <p>{{ projects[activeSlide].description }}</p>

                                <div class="d-flex flex-wrap gap-3">

                                    <div v-for="technology in projects[activeSlide].technologies" :key="technology.id" class="badge" :class="technology.bg_color">
                                        {{ technology.name }}
                                    </div>

                                </div>
                            </div>

                            <div v-else class="h-100 d-flex flex-column justify-content-center align-items-center gap-4 text-center">
                                <span>Nessun progetto presente nel database</span>
                            </div>



                            <div v-if="dim > 1" @click.stop="goDown()" class="al-rounded down">
                                <i class="fa-solid fa-chevron-down"></i>
                            </div>

                            <div class="right-dots">

                                <i v-if="dim > 0" @click.stop="goTo(0)" :class="[{ 'fa-solid': activeSlide == 0 }, { 'fa-regular': activeSlide != 0 }]" class="fa-circle"></i>
                                <i v-if="dim > 1" @click.stop="goTo(1)" :class="[{ 'fa-solid': activeSlide == 1 }, { 'fa-regular': activeSlide != 1 }]" class="fa-circle"></i>
                                <i v-if="dim > 2" @click.stop="goTo(2)" :class="[{ 'fa-solid': activeSlide == 2 }, { 'fa-regular': activeSlide != 2 }]" class="fa-circle"></i>


                            </div>
                        </div>

                    </div>

                </div>
            </div>

        </div>

        <AppFooter />
    </div>
</template>

<script>
import axios from "axios";
import { store } from "../store.js";

import AppHeader from '../components/AppHeader.vue';
import AppFooter from "../components/AppFooter.vue";
import Loader from "../components/Loader.vue";

export default {
    name: "HomePage",

    data() {
        return {
            store,
            projects: null,
            dim: null,
            activeSlide: 0,
            timeout: null,
            loading: true
        }
    },

    components: {
        AppHeader,
        AppFooter,
        Loader
    },

    methods: {
        queryServer() {
            axios.get(store.apiUrl + "/carousel").then((response) => {
                this.projects = response.data.results;
                this.dim = this.projects.length;
            }).catch((error) => {
                console.log(error);
            }).finally(() => {
                this.loading = false;
            });
        },

        goUp() {
            this.resetTimeout();

            this.activeSlide++;
            if (this.activeSlide == 3 || this.activeSlide >= this.dim)
                this.activeSlide = 0;
        },

        goDown() {
            this.resetTimeout();

            this.activeSlide--;
            if (this.activeSlide == -1)
                this.activeSlide = 2;
        },

        goTo(index) {
            this.resetTimeout();

            this.activeSlide = index;
        },

        resetTimeout() {
            clearTimeout(this.timeout);
            this.timeout = setTimeout(this.goUp, 7000);
        }
    },

    beforeMount() {
        this.queryServer();
        this.timeout = setTimeout(this.goUp, 7000);
    }
}
</script>

<style lang="scss" scoped>
@use "../assets//styles/partials/variables.scss" as *;

.opacity-effect {
    animation: opacity-animation 2.5s linear;
}

@keyframes opacity-animation {
    from {
        opacity: 0;
    }

    to {
        opacity: 1;
    }
}

.bg-content {
    height: 100%;
    background-color: $header_color;
    padding: 1.5rem;
    border-radius: 6px;
}

.book {
    display: block;
    width: 50%;
}

.al-carousel {
    height: 100%;
    position: relative;
    padding: 5rem 0;
}

.al-rounded {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    justify-content: center;
    align-items: center;
    width: 35px;
    height: 35px;
    border: 1px solid $text_color;
    border-radius: 50%;
    transition: transform 300ms;
    cursor: pointer;

    &:hover {
        transform: scale(1.1) translateX(-50%);
    }
}

.al-rounded.up {
    top: 0;
}

.al-rounded.down {
    bottom: 0;
}

.right-dots {
    position: absolute;
    bottom: 0;
    right: 0;
    display: flex;
    gap: 0.5rem;
}

.fa-circle {
    cursor: pointer;
}
</style>