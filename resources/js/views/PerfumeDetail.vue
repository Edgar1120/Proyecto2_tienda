<template>
  <div class="container mt-5" v-if="perfume && perfume.precio !== undefined">
    <div class="card shadow p-4">
      <div class="row">
        <div class="col-md-5">
          <img :src="perfume.imagen" class="img-fluid rounded" :alt="perfume.nombre">
        </div>
        <div class="col-md-7">
          <h2 class="text-purple">{{ perfume.nombre }}</h2>
          <h4 class="text-muted">{{ perfume.marca }}</h4>

          <p class="mt-3">
            <strong>Género:</strong> {{ perfume.genero }} <br>
            <strong>Volumen:</strong> {{ perfume.volumen }} <br>
            <strong>Categoría:</strong> {{ perfume.categoria?.nombre }} <br>
            <strong>Precio:</strong> ${{ perfume.precio.toFixed(2) }} <br>
            <strong>Etiquetas:</strong> {{ perfume.etiquetas }} <br>
            <strong>Envío Gratis:</strong> 
            <span :class="perfume.envio_gratis ? 'text-success' : 'text-danger'">
              {{ perfume.envio_gratis ? 'Sí' : 'No' }}
            </span>
          </p>

          <router-link :to="`/perfumes/${perfume.id}/edit`" class="btn btn-outline-primary me-2">
            Editar
          </router-link>

          <button @click="eliminarPerfume" class="btn btn-outline-danger me-2">
            Eliminar
          </button>

          <router-link to="/perfumes" class="btn btn-secondary">
            Volver
          </router-link>
        </div>
      </div>
    </div>
  </div>

  <div v-else class="text-center mt-5">
    <p>Cargando perfume...</p>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRoute, useRouter } from 'vue-router';
import axios from 'axios';

const route = useRoute();
const router = useRouter();
const perfume = ref(null);

onMounted(async () => {
  try {
    const res = await axios.get(`/api/perfumes/${route.params.id}`);
    perfume.value = res.data.data;
  } catch (error) {
    console.error('Error al cargar perfume:', error);
    perfume.value = null;
  }
});

const eliminarPerfume = async () => {
  if (confirm('¿Estás seguro de que deseas eliminar este perfume?')) {
    try {
      await axios.delete(`/api/perfumes/${perfume.value.id}`);
      alert('Perfume eliminado correctamente.');
      router.push('/perfumes');
    } catch (error) {
      console.error('Error al eliminar perfume:', error);
      alert('Error al eliminar el perfume.');
    }
  }
};
</script>

<style scoped>
.text-purple {
  color: #6f42c1;
}
</style>
