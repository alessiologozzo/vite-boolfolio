<template>
    <header>
        <div class="container h-100 d-flex justify-content-between">

            <div class="d-flex align-items-center gap-3">
                <router-link :to="{ name: 'HomePage' }">
                    <img src="../assets/img/logo.png" alt="logo" class="logo">
                </router-link>

                <router-link :to="{ name: 'HomePage' }">
                    <span>Home</span>
                </router-link>
            </div>

            <div class="d-none d-md-flex">

                <router-link :to="{ name: 'ProjectPage' }" class="h-100 d-flex align-items-center hover-effect px-3">Progetti</router-link>

                <a href="http://localhost:8000/admin" class="h-100 d-flex align-items-center hover-effect px-3">Login</a>

            </div>

            <div @click.stop="showMenu($event)" class="al-menu-data position-relative d-flex align-items-center d-md-none cursor-pointer">

                <i class="fa-solid fa-bars fs-2"></i>

                <div class="al-menu">

                    <router-link :to="{ name: 'ProjectPage' }" class="al-menu-item">
                        Progetti
                    </router-link>

                    <a href="http://localhost:8000/admin" class="al-menu-item">
                        Login
                    </a>

                </div>
            </div>
        </div>
    </header>
</template>

<script>
import HomePage from '../pages/HomePage.vue';

export default {
    name: "HeaderApp",

    methods: {

        showMenu(e) {
            let menu = e.currentTarget.querySelector(".al-menu");
            let menuData = e.currentTarget.closest(".al-menu-data");
            let height = menuData.offsetHeight;

            menu.classList.toggle("al-menu-visible");
            menu.style.top = `${height}px`;
        },

        removeMenusHandler() {
            let menus = document.getElementsByClassName("al-menu");

            for (let i = 0; i < menus.length; i++)
                menus[i].classList.remove("al-menu-visible");
        }
    },

    created() {
        document.addEventListener("click", this.removeMenusHandler);
    },
}
</script>

<style lang="scss" scoped>
@use "../assets/styles/partials/variables.scss" as *;

header {
    background-color: $header_color;
    border-bottom: 1px solid black;
    padding: 1rem;
}

.logo {
    display: block;
    width: 250px;
}

a {
    color: white;
    text-decoration: none;
}

.hover-effect {
    position: relative;
    background-color: $header_color;
    transition: background-color 300ms;
    cursor: pointer;

    &::after {
        content: "";
        position: absolute;
        top: 50%;
        left: 0;
        width: 1px;
        height: 0;
        transition: all 300ms;
        background-color: $text_color;
    }

    &:hover {
        background-color: $header_color_hover;
    }

    &:hover::after {
        top: 0;
        height: 100%;
    }
}

.al-menu {
    // transform-origin: top right;
    transform: scale(0);
    position: absolute;
    right: 0;
    // transition: transform 200ms;
    border-radius: 5px;
    border: 1px solid grey;
    background-color: white;
    z-index: 100;
}

.al-menu-item {
    display: block;
    white-space: nowrap;
    background-color: white;
    padding: 10px 55px 10px 18px;
    cursor: pointer;
    transition: background-color 200ms;
    border-radius: 5px;
    z-index: 10;
    color: black;

    a {
        color: black;
    }
}

.al-menu-item:hover {
    background-color: lightgray;
}

.al-menu-visible {
    transform: scale(1);
}

.cursor-pointer {
    cursor: pointer;
}
</style>