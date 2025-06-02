<template>
    <div class="welcome-page text-white d-flex align-items-center justify-content-center">
        <div class="overlay"></div>
        <div class="content text-center">
            <h1 class="display-4 fw-bold">Bienvenid@ a Perfumes Wong</h1>
            <p class="lead mt-3">Explora nuestra colección de perfumes exclusivos para cada estilo y ocasión.</p>
            <router-link to="/perfumes" class="btn btn-purple btn-lg mt-4">Ver Catálogo</router-link>
        </div>
    </div>
</template>

<script setup>

</script>

<style scoped>
.welcome-page {
    position: relative;
    height: 100vh;
    background-size: cover;
    background-position: center;
    overflow: hidden;
}

.overlay {
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    background: rgba(39, 0, 66, 0.6);
  
    z-index: 1;
}

.content {
    position: relative;
    z-index: 2;
    max-width: 600px;
    padding: 20px;
}


.btn-purple {
    background-color: #6f42c1;
    color: white;
    border: none;
}

.btn-purple:hover {
    background-color: #5a35a0;
    color: white;
}
</style>
