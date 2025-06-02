<template>
  <div class="container py-5">
    <!-- Título Principal con nuevo color -->
    <h2 class="mb-4 text-center" style="color: #6f42c1;">Gestión de Categorías</h2>

    <!-- Formulario de Categoría -->
    <div class="card shadow-sm mb-4">
      <!-- Encabezado de la tarjeta con color personalizado -->
      <div class="card-header text-white" style="background-color: #6f42c1;">
        {{ editando ? 'Editar Categoría' : 'Nueva Categoría' }}
      </div>
      <div class="card-body">
        <form @submit.prevent="guardarCategoria">
          <div class="mb-3">
            <label class="form-label">Nombre</label>
            <input
              v-model="form.nombre"
              type="text"
              class="form-control"
              placeholder="Ej. Fragancias de lujo"
              required
            />
          </div>
          <div class="d-flex justify-content-end">
            <!-- Botón con color personalizado -->
            <button type="submit" class="btn text-white me-2" style="background-color: #6f42c1;">
              {{ editando ? 'Actualizar' : 'Crear' }}
            </button>
            <button
              v-if="editando"
              type="button"
              @click="cancelarEditar"
              class="btn btn-secondary"
            >
              Cancelar
            </button>
          </div>
        </form>
      </div>
    </div>

    <!-- Lista de Categorías -->
    <div v-if="categorias.length" class="card shadow-sm">
      <div class="card-header bg-light">
        <strong>Categorías registradas</strong>
      </div>
      <ul class="list-group list-group-flush">
        <li
          v-for="categoria in categorias"
          :key="categoria.id"
          class="list-group-item d-flex justify-content-between align-items-center"
        >
          <span>{{ categoria.nombre }}</span>
          <div>
            <button class="btn btn-sm btn-outline-primary me-2" @click="editarCategoria(categoria)">
              <i class="bi bi-pencil"></i> Editar
            </button>
            <button class="btn btn-sm btn-outline-danger" @click="eliminarCategoria(categoria.id)">
              <i class="bi bi-trash"></i> Eliminar
            </button>
          </div>
        </li>
      </ul>
    </div>

    <div v-else class="alert alert-warning text-center">
      No hay categorías disponibles.
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const categorias = ref([]);
const form = ref({ nombre: '' });
const editando = ref(false);
const idEditando = ref(null);

const obtenerCategorias = async () => {
  const res = await axios.get('/api/categorias');
  categorias.value = res.data.data;
};

const guardarCategoria = async () => {
  if (editando.value) {
    await axios.put(`/api/categorias/${idEditando.value}`, form.value);
  } else {
    await axios.post('/api/categorias', form.value);
  }

  await obtenerCategorias();
  cancelarEditar();
};

const editarCategoria = (categoria) => {
  form.value.nombre = categoria.nombre;
  idEditando.value = categoria.id;
  editando.value = true;
};

const cancelarEditar = () => {
  form.value.nombre = '';
  idEditando.value = null;
  editando.value = false;
};

const eliminarCategoria = async (id) => {
  if (confirm('¿Deseas eliminar esta categoría?')) {
    await axios.delete(`/api/categorias/${id}`);
    await obtenerCategorias();
  }
};

onMounted(obtenerCategorias);
</script>

<!-- Bootstrap Icons -->
<link
  href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.10.5/font/bootstrap-icons.css"
  rel="stylesheet"
/>
