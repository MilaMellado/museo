<script setup>
// 1. Título directo en la página
useHead({
  title: 'Cleveland Vault | Galería de Arte',
  meta: [
    { name: 'description', content: 'Ficha técnica y galería del Museo de Cleveland' }
  ]
})

const artworks = ref([]);
const skip = ref(0);
const limit = 12;
const pending = ref(false);
const selectedArt = ref(null);

const loadArtworks = async () => {
  if (pending.value) return;
  pending.value = true;
  try {
    const data = await $fetch('https://openaccess-api.clevelandart.org/api/artworks/', {
      query: { skip: skip.value, limit: limit, has_image: 1, type: 'Painting' }
    });
    artworks.value.push(...data.data);
    skip.value += limit;
  } catch (err) {
    console.error("Error:", err);
  } finally {
    pending.value = false;
  }
};

onMounted(() => loadArtworks());
</script>

<template>
  <div class="museum-app">
    <TheNavbar />

    <main class="container">
      <div class="grid">
        <div v-for="art in artworks" :key="art.id" class="card" @click="selectedArt = art">
          <div class="image-wrapper">
            <img :src="art.images.web.url" :alt="art.title" loading="lazy" />
            <div class="overlay"><span>Explorar</span></div>
          </div>
          <div class="simple-info">
            <h3>{{ art.title }}</h3>
            <p>{{ art.creators[0]?.description || 'Autor Desconocido' }}</p>
          </div>
        </div>
      </div>

      <div class="load-more-container">
        <button @click="loadArtworks" :disabled="pending" class="load-btn">
          {{ pending ? 'Buscando...' : 'Cargar más obras' }}
        </button>
      </div>
    </main>

    <Transition name="fade">
      <ArtDetailsModal 
        v-if="selectedArt" 
        :art="selectedArt" 
        @close="selectedArt = null" 
      />
    </Transition>
  </div>
</template>

<style scoped>
/* FONDO DIRECTO EN EL INDEX */
.museum-app {
  min-height: 100vh;
  /* Gradiente radial fijo que no se mueve al hacer scroll */
  background: radial-gradient(circle at 50% 0%, #1e1e1e 0%, #050505 70%) fixed;
  color: #fff;
  font-family: 'Inter', sans-serif;
}

.container { max-width: 1400px; margin: 0 auto; padding: 40px 20px; }

/* El resto de tus estilos del grid y cards... */
.grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(300px, 1fr)); gap: 35px; }
.card { background: #111; cursor: pointer; transition: 0.3s; border: 1px solid #1a1a1a; }
.card:hover { transform: translateY(-8px); border-color: #333; }
.image-wrapper { position: relative; aspect-ratio: 4/5; overflow: hidden; }
.image-wrapper img { width: 100%; height: 100%; object-fit: cover; }
.overlay { position: absolute; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.7); display: flex; align-items: center; justify-content: center; opacity: 0; transition: 0.3s; }
.card:hover .overlay { opacity: 1; }
.simple-info { padding: 20px; }
.load-more-container { text-align: center; margin-top: 60px; }
.load-btn { background: white; color: black; border: none; padding: 15px 40px; border-radius: 4px; font-weight: 800; cursor: pointer; }
.fade-enter-active, .fade-leave-active { transition: opacity 0.4s ease; }
.fade-enter-from, .fade-leave-to { opacity: 0; }
</style>