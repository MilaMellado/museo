<script setup>
import { ref, onMounted } from 'vue';

const artworks = ref([]);
const skip = ref(0);
const limit = 12;
const pending = ref(false);
const selectedArt = ref(null);

// Función para cargar imágenes de la API de Cleveland
const loadArtworks = async () => {
  if (pending.value) return;
  pending.value = true;
  
  try {
    const data = await $fetch('https://openaccess-api.clevelandart.org/api/artworks/', {
      query: {
        skip: skip.value,
        limit: limit,
        has_image: 1,
        type: 'Painting'
      }
    });
    
    artworks.value.push(...data.data);
    skip.value += limit;
  } catch (err) {
    console.error("Error cargando el museo:", err);
  } finally {
    pending.value = false;
  }
};

onMounted(() => {
  loadArtworks();
});

const openDetails = (art) => {
  selectedArt.value = art;
};

const closeDetails = () => {
  selectedArt.value = null;
};
</script>

<template>
  <div class="museum-app">
    <nav class="navbar">
      <div class="logo">CLEVELAND<span>GALLERY</span></div>
    </nav>

    <main class="container">
      <div class="grid">
        <div v-for="art in artworks" :key="art.id" class="card" @click="openDetails(art)">
          <div class="image-wrapper">
            <img :src="art.images.web.url" :alt="art.title" loading="lazy" />
            <div class="overlay">
              <span>Explorar Obra</span>
            </div>
          </div>
          <div class="simple-info">
            <h3>{{ art.title }}</h3>
            <p>{{ art.creators[0]?.description || 'Autor Desconocido' }}</p>
          </div>
        </div>
      </div>

      <div class="load-more-container">
        <button @click="loadArtworks" :disabled="pending" class="load-btn">
          {{ pending ? 'Buscando en el archivo...' : 'Cargar más obras' }}
        </button>
      </div>
    </main>

    <Transition name="fade">
      <div v-if="selectedArt" class="modal-overlay" @click.self="closeDetails">
        <div class="modal-content">
          <button class="close-btn" @click="closeDetails">×</button>
          
          <div class="modal-body">
            <div class="modal-image">
              <img :src="selectedArt.images.web.url" :alt="selectedArt.title" />
            </div>
            
            <div class="modal-text">
              <span class="accession">N° Registro: {{ selectedArt.accession_number }}</span>
              <h2>{{ selectedArt.title }}</h2>
              <p class="author-large">{{ selectedArt.creators[0]?.description }}</p>
              
              <div class="info-grid">
                <div class="info-item">
                  <strong>Técnica</strong>
                  <p>{{ selectedArt.technique || 'Óleo / Técnica mixta' }}</p>
                </div>
                <div class="info-item">
                  <strong>Fecha</strong>
                  <p>{{ selectedArt.creation_date || 'Sin fecha registrada' }}</p>
                </div>
                <div class="info-item">
                  <strong>Ubicación</strong>
                  <p>{{ selectedArt.department }}</p>
                </div>
              </div>

              <div class="description-box">
                <strong>Contexto Histórico</strong>
                <p>{{ selectedArt.wall_description || 'Esta pieza es una parte fundamental de la colección. Representa las tendencias artísticas de su época y destaca por su maestría técnica y composición única.' }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </Transition>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;800&display=swap');

.museum-app {
  background-color: #080808;
  color: #fff;
  min-height: 100vh;
  font-family: 'Inter', sans-serif;
}

.navbar {
  padding: 20px 40px;
  background: rgba(8, 8, 8, 0.9);
  backdrop-filter: blur(15px);
  border-bottom: 1px solid #1a1a1a;
  position: sticky;
  top: 0;
  z-index: 50;
}

.logo { font-weight: 800; letter-spacing: 5px; font-size: 1.2rem; }
.logo span { font-weight: 300; color: #555; }

.container { max-width: 1400px; margin: 0 auto; padding: 40px 20px; }

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 35px;
}

.card { background: #111; cursor: pointer; transition: transform 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275); }
.card:hover { transform: translateY(-8px); }

.image-wrapper { position: relative; aspect-ratio: 4/5; overflow: hidden; background: #1a1a1a; }
.image-wrapper img { width: 100%; height: 100%; object-fit: cover; }

.overlay {
  position: absolute; top: 0; left: 0; width: 100%; height: 100%;
  background: rgba(0,0,0,0.7); display: flex; align-items: center; justify-content: center;
  opacity: 0; transition: 0.3s;
}

.card:hover .overlay { opacity: 1; }
.overlay span { border: 1px solid white; padding: 10px 20px; text-transform: uppercase; font-size: 0.7rem; letter-spacing: 2px; }

.simple-info { padding: 20px; }
.simple-info h3 { font-size: 1rem; margin-bottom: 5px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.simple-info p { font-size: 0.8rem; color: #777; }

.load-more-container { text-align: center; margin-top: 60px; }
.load-btn {
  background: white; color: black; border: none; padding: 15px 40px;
  border-radius: 4px; font-weight: 800; cursor: pointer; text-transform: uppercase; letter-spacing: 1px;
}

.modal-overlay {
  position: fixed; top: 0; left: 0; width: 100%; height: 100%;
  background: rgba(0,0,0,0.9); z-index: 100;
  display: flex; align-items: center; justify-content: center; padding: 20px;
}

.modal-content {
  background: #111; width: 100%; max-width: 1100px;
  max-height: 90vh; overflow-y: auto; position: relative;
  border: 1px solid #333;
}

.modal-body { display: grid; grid-template-columns: 1fr 1fr; gap: 40px; padding: 50px; }
.modal-image img { width: 100%; border-radius: 2px; box-shadow: 0 10px 30px rgba(0,0,0,0.5); }

.accession { color: #555; font-size: 0.7rem; letter-spacing: 2px; }
.modal-text h2 { font-size: 2.5rem; margin: 15px 0; font-weight: 800; line-height: 1; }
.author-large { font-size: 1.2rem; color: #aaa; margin-bottom: 40px; }

.info-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 30px; margin-bottom: 40px; }
.info-item strong { font-size: 0.6rem; text-transform: uppercase; color: #444; letter-spacing: 2px; display: block; margin-bottom: 5px; }
.info-item p { font-size: 1rem; color: #ddd; }

.description-box { border-top: 1px solid #222; padding-top: 30px; }
.description-box strong { font-size: 0.6rem; text-transform: uppercase; color: #444; letter-spacing: 2px; display: block; margin-bottom: 15px; }
.description-box p { font-size: 1rem; line-height: 1.7; color: #999; }

.close-btn { position: absolute; top: 20px; right: 25px; background: none; border: none; color: white; font-size: 2.5rem; cursor: pointer; }

.fade-enter-active, .fade-leave-active { transition: opacity 0.4s ease; }
.fade-enter-from, .fade-leave-to { opacity: 0; }

@media (max-width: 900px) {
  .modal-body { grid-template-columns: 1fr; padding: 30px; }
}
</style>