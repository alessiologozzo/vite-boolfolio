<template>
    <div class="loader">
        <i class="fa-solid fa-gear spinner"></i>
    </div>
</template>

<script>
    export default {
        
    }
</script>

<style lang="scss" scoped>
.loader{
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 5rem;
}
.spinner{
    animation: spin 2s linear infinite;
}

@keyframes spin {
    from{
        transform: rotate(0);
    }

    to{
        transform: rotate(359deg);
    }
}
</style>