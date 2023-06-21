<template>
    <Loader v-if="loading" />

    <div v-else>
        <AppHeader />

        <div class="container">
            <h1 class="py-3 text-center">Progetti</h1>

            <div class="d-flex justify-content-end">

                <div class="d-flex flex-column al-card gap-3">
                    <h5 class="text-center">Filtri</h5>

                    <div class="d-flex flex-wrap gap-3">

                        <div class="d-flex flex-column gap-1">
                            <label for="nameSearch">Cerca per titolo</label>
                            <input type="text" id="nameSearch" name="nameSearch" placeholder="Titolo..." v-model="nameSearch">
                        </div>

                        <div class="d-flex flex-column gap-1">
                            <label for="technologySearch">Cerca per tecnologia</label>
                            <select name="technologySearch" id="technologySearch" v-model="technologySearch">

                                <option value="" selected>Tutte</option>

                                <option v-for="technology in technologies" :key="technology.id" :value="technology.name">
                                    {{ technology.name }}
                                </option>

                            </select>
                        </div>

                    </div>

                    <div class="d-flex justify-content-end align-items-center">
                        <div @click="resetFilter()" class="al-button">Reset</div>
                    </div>
                </div>

            </div>

            <div class="row py-5 mb-5">
                <div v-show="searchByName(project.name) && searchByTechnology(project.technologies)" v-for="project in projects.data" :key="project.id" class="col-12 col-sm-6 col-md-4 col-lg-3 gy-5">

                    <div class="al-card">
                        <h5>{{ project.name }}</h5>

                        <p>{{ project.description }}</p>

                        <div class="d-flex justify-content-center align-items-center flex-wrap gap-3">

                            <span v-for="technology in project.technologies" :key="technology.id" class="badge" :class="technology.bg_color">
                                {{ technology.name }}
                            </span>

                        </div>

                    </div>
                </div>


                <!-- paginazione -->
                <div v-if="projects.links <= 3" class="col-12 d-flex pt-5">
                    <div v-for="(link, index) in projects.links" :key="index">

                        <div v-if="index == 0" class="al-square" @click="queryServer(link.url)">
                            <i class="fa-solid fa-chevron-left"></i>
                        </div>

                        <div v-else-if="index == projects.links.length - 1" class="al-square" @click="queryServer(link.url)">
                            <i class="fa-solid fa-chevron-right"></i>
                        </div>

                        <div v-else-if="index == 1 || index == projects.last_page || (index >= projects.current_page - 2 && index <= projects.current_page + 2)" class="al-square" :class="{ 'al-current-page': index == projects.current_page }" @click="queryServer(link.url)">
                            <i>{{ link.label }}</i>
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
    name: "ProjectPage",

    data() {
        return {
            store,
            projects: null,
            technologies: null,
            loading: true,
            nameSearch: "",
            technologySearch: ""
        }
    },

    components: {
        AppHeader,
        AppFooter,
        Loader
    },

    methods: {
        queryServer(url) {
            if (url != null)
                axios.get(url).then((response) => {
                    this.projects = response.data.projects;
                    this.technologies = response.data.technologies;
                }).catch((error) => {
                    console.log(error);
                }).finally(() => {
                    this.loading = false;
                });
        },

        searchByName(name) {
            let result = false;

            if (name.toUpperCase().match(this.nameSearch.toUpperCase()) != null)
                result = true;

            return result;
        },

        searchByTechnology(technologies) {
            let result = false;
            for (let i = 0; i < technologies.length && !result; i++)
                if (technologies[i].name.match(this.technologySearch) != null)
                    result = true;

            return result;
        },

        resetFilter() {
            this.nameSearch = "";
            this.technologySearch = "";
        }
    },

    created() {
        this.queryServer(store.apiUrl + "/index");
    },
}
</script>

<style lang="scss" scoped>
@use "../assets/styles/partials/variables.scss" as *;

.bg_content {
    background-color: $header_color;
}

.al-card {
    height: 100%;
    display: flex;
    flex-flow: column nowrap;
    justify-content: center;
    gap: 0.5rem;
    padding: 1.5rem;
    background-color: $header_color;
    border: 1px solid black;
    border-radius: 6px;
    text-align: center;
}

.al-square {
    height: 40px;
    width: 40px;
    display: flex;
    justify-content: center;
    align-items: center;
    border: 1px solid rgb(148, 148, 148);
    font-size: 1.2rem;
    cursor: pointer;
    background-color: $header_color;
    transition: all 300ms;

    &:hover {
        background-color: rgb(100, 100, 100);
        border: 1px solid white;
    }
}

.al-current-page {
    background-color: rgb(54, 164, 255);

    &:hover {
        background-color: rgb(54, 164, 255);
        ;
    }
}

input[type="text"],
select {
    width: 100%;
    height: 30px;
    background-color: white;
    color: black;
    border: 1px solid black;
    border-radius: 4px;
    padding: 0.3rem;
}

.al-button {
    padding: 0.5rem 1.7rem;
    color: black;
    font-style: italic;
    background-color: gold;
    border: 3px groove goldenrod;
    transition: transform 300ms;
    cursor: pointer;

    &:hover {
        color: black;
        transform: scale(1.1);
    }
}
</style>