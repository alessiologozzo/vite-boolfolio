<template>
    <footer>

        <div class="container d-flex justify-content-end align-items-center gap-3">

            <i class="fa-brands fa-facebook"></i>
            <i class="fa-brands fa-twitter"></i>
            <i class="fa-brands fa-linkedin"></i>
            <i class="fa-brands fa-tiktok"></i>
            <i class="fa-brands fa-youtube"></i>


        </div>

    </footer>
</template>

<script>
    export default {
        name: "AppFooter"
    }
</script>

<style lang="scss" scoped>
@use "../assets/styles/partials/variables.scss" as *;

    footer{
        background-color: $header_color;
        border-top: 1px solid black;
        padding: 2rem;
    }

    i{
        font-size: 1.3rem;
        opacity: 0.5;
    }
</style>