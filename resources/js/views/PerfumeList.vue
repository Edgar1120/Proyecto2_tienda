<template>
  <div class="container my-4">
    <div class="d-flex justify-content-between align-items-center mb-4">
      <h3>Lista de Perfumes</h3>
      <router-link to="/perfumes/new" class="btn btn-purple">Agregar Perfume</router-link>
    </div>

    <!-- 🔍 Input de búsqueda -->
    <div class="mb-4">
      <input type="text" class="form-control" v-model="searchTerm" placeholder="Buscar por nombre o marca..."
        @input="onSearch" />
    </div>

    <!-- 🧴 Lista de perfumes -->
    <div class="row">
      <div class="col-md-4 mb-4" v-for="perfume in perfumes" :key="perfume.id">
        <PerfumeCard :perfume="perfume" />
      </div>
    </div>

    <!-- ⏩ Paginación -->
    <div class="d-flex justify-content-center align-items-center mt-4 flex-wrap gap-2">
      <button class="btn btn-purple-outline" :disabled="currentPage === 1" @click="changePage(currentPage - 1)">
        Anterior
      </button>

      <button v-for="page in totalPages" :key="page" class="btn"
        :class="page === currentPage ? 'btn-purple' : 'btn-purple-outline'" @click="changePage(page)">
        {{ page }}
      </button>

      <button class="btn btn-purple-outline" :disabled="currentPage === totalPages"
        @click="changePage(currentPage + 1)">
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
const searchTerm = ref('');

// Cargar perfumes con búsqueda
const fetchPerfumes = async (page = 1) => {
  try {
    const res = await axios.get('/api/perfumes', {
      params: {
        page: page,
        buscar: searchTerm.value
      }
    });

    const pagination = res.data.data;
    perfumes.value = pagination.data ?? [];
    currentPage.value = pagination.current_page;
    totalPages.value = pagination.last_page;
  } catch (error) {
    console.error('Error al cargar perfumes:', error);
  }
};

// Cambiar página
const changePage = (page) => {
  if (page >= 1 && page <= totalPages.value) {
    fetchPerfumes(page);
  }
};

// Ejecutar búsqueda (reinicia a página 1)
const onSearch = () => {
  fetchPerfumes(1);
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
