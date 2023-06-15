<template>

<AppMain/>

</template>

<script>
import axios from "axios";
import { RouterLink, RouterView } from 'vue-router'
import AppMain from './components/AppMain.vue'

  export default {
    name: "App",

    components: {
      AppMain
    },

    methods: {
        queryServer() {



            axios.get("http://127.0.0.1:8000/api/").then((response) => {
                console.log(response.data);
            });

        },
    },

    mounted() {
        this.queryServer();
    }

  }
</script>

<style lang="scss" scoped>

</style>