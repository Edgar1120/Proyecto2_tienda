<template>
  <div class="perfume-card card shadow-sm rounded p-2 mb-3">
    <!-- Imagen clicable -->
    <router-link :to="`/perfumes/${perfume.id}`">
      <img :src="perfume.imagen" class="card-img-top" :alt="perfume.nombre" height="180">
    </router-link>

    <div class="card-body d-flex flex-column p-2">
      <h6 class="card-title mb-1">{{ perfume.marca }}</h6>

      <!-- Nombre clicable -->
      <router-link 
        :to="`/perfumes/${perfume.id}`" 
        class="card-text fw-bold text-purple text-decoration-none mb-1 small"
      >
        {{ perfume.nombre }}
      </router-link>

      <p class="mb-1 small">Género: {{ perfume.genero }}</p>
      <p class="mb-1 small">Volumen: {{ perfume.volumen }}</p>
      <p class="mb-1 small">Categoría: {{ perfume.categoria.nombre }}</p>
      <p class="fw-bold text-success small">${{ perfume.precio.toFixed(2) }}</p>

      <!-- Botón Ver más -->
      <router-link 
        :to="`/perfumes/${perfume.id}`" 
        class="btn btn-sm btn-purple mt-auto align-self-start"
      >
        Ver más
      </router-link>
    </div>
  </div>
</template>

<script setup>
defineProps(['perfume']);
</script>

<style scoped>
.perfume-card {
  max-width: 500px;
  font-size: 1rem; /* Tamaño general más pequeño */
  margin-right: 10px;
}

.text-purple {
  color: #6f42c1;
}

.btn-purple {
  background-color: #6f42c1;
  color: white;
  padding: 0.25rem 0.75rem;
  font-size: 0.8rem;
}
.btn-purple:hover {
  background-color: #5a359d;
  color: white;
}
</style>
