<template>
  <div class="container mt-5">
    <h3>{{ isEdit ? 'Editar' : 'Agregar' }} Perfume</h3>
    <form @submit.prevent="submit">
      <div class="mb-3">
        <label>Nombre</label>
        <input v-model="form.nombre" class="form-control" required>
      </div>
      <div class="mb-3">
        <label>Marca</label>
        <input v-model="form.marca" class="form-control" required>
      </div>
      <div class="mb-3">
        <label>Precio</label>
        <input type="number" v-model="form.precio" class="form-control" required>
      </div>
      <div class="mb-3">
        <label>Género</label>
        <select v-model="form.genero" class="form-select" required>
          <option>Hombre</option>
          <option>Mujer</option>
          <option>Unisex</option>
        </select>
      </div>
      <div class="mb-3">
        <label>Volumen</label>
        <input v-model="form.volumen" class="form-control" required>
      </div>
      <div class="mb-3">
        <label>Categoría</label>
        <select v-model="form.id_categoria" class="form-select" required>
          <option v-for="cat in categorias" :key="cat.id" :value="cat.id">
            {{ cat.nombre }}
          </option>
        </select>
      </div>
      <div class="mb-3">
        <label>Imagen (URL)</label>
        <input v-model="form.imagen" class="form-control">
      </div>
      <div class="mb-3">
        <label>Etiquetas</label>
        <input v-model="form.etiquetas" class="form-control">
      </div>
      <div class="form-check mb-3">
        <input class="form-check-input" type="checkbox" v-model="form.envio_gratis">
        <label class="form-check-label">Envío Gratis</label>
      </div>

      <button class="btn btn-success">{{ isEdit ? 'Actualizar' : 'Guardar' }}</button>
      <router-link to="/perfumes" class="btn btn-secondary ms-2">Cancelar</router-link>
    </form>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRoute, useRouter } from 'vue-router';
import axios from 'axios';

const route = useRoute();
const router = useRouter();
const isEdit = !!route.params.id;

const form = ref({
  nombre: '',
  marca: '',
  precio: '',
  volumen: '',
  genero: '',
  imagen: '',
  categoria_id: '',
  envio_gratis: false,
  etiquetas: '',
  rating: 0
});


const categorias = ref([]);

const loadCategorias = async () => {
  const res = await axios.get('/api/categorias');
  categorias.value = res.data.data;
};

onMounted(async () => {
  await loadCategorias();
  if (isEdit) {
    const res = await axios.get(`/api/perfumes/${route.params.id}`);
    Object.assign(form.value, res.data.data);
  }
});

const submit = async () => {
  if (isEdit) {
    await axios.put(`/api/perfumes/${route.params.id}`, form.value);
  } else {
    await axios.post('/api/perfumes', form.value);
  }
  router.push('/perfumes');
};
</script>
