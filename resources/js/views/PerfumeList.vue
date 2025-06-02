<template>
  <div class="container my-4">
    <div class="d-flex justify-content-between align-items-center mb-4">
      <h3>Lista de Perfumes</h3>
      <router-link to="/perfumes/new" class="btn btn-purple">Agregar Perfume</router-link>
    </div>

    <div class="row">
      <div class="col-md-4 mb-4" v-for="perfume in perfumes" :key="perfume.id">
        <PerfumeCard :perfume="perfume" />
      </div>
    </div>

    <!-- Paginación -->
    <div class="d-flex justify-content-center align-items-center mt-4 flex-wrap gap-2">
  <!-- Botón anterior -->
  <button 
    class="btn btn-purple-outline"
    :disabled="currentPage === 1"
    @click="changePage(currentPage - 1)"
  >
    Anterior
  </button>

  <!-- Botones numerados -->
  <button 
    v-for="page in totalPages" 
    :key="page"
    class="btn"
    :class="page === currentPage ? 'btn-purple' : 'btn-purple-outline'"
    @click="changePage(page)"
  >
    {{ page }}
  </button>

  <!-- Botón siguiente -->
  <button 
    class="btn btn-purple-outline"
    :disabled="currentPage === totalPages"
    @click="changePage(currentPage + 1)"
  >
    Siguiente
  </button>
</div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import axios from 'axios';
import PerfumeCard from '../components/PerfumeCard.vue';

const perfumes = ref([]);
const currentPage = ref(1);
const totalPages = ref(1);

const fetchPerfumes = async (page = 1) => {
  try {
    const res = await axios.get(`/api/perfumes?page=${page}`);
    const pagination = res.data.data;

    perfumes.value = pagination.data ?? [];
    currentPage.value = pagination.current_page;
    totalPages.value = pagination.last_page;
  } catch (error) {
    console.error('Error al cargar perfumes:', error);
  }
};

const changePage = (page) => {
  if (page >= 1 && page <= totalPages.value) {
    fetchPerfumes(page);
  }
};

onMounted(() => fetchPerfumes());
</script>

<style scoped>
.btn-purple {
  background-color: #6f42c1;
  color: white;
}
.btn-purple:hover {
  background-color: #5a35a0;
  color: white;
}
.btn-purple-outline {
  border: 1px solid #6f42c1;
  color: #6f42c1;
  background-color: transparent;
}
.btn-purple-outline:hover {
  background-color: #6f42c1;
  color: white;
}
</style>
