<script setup>
defineProps(['art']);
defineEmits(['close']);
</script>

<template>
  <div class="modal-overlay" @click.self="$emit('close')">
    <div class="modal-content">
      <button class="close-btn" @click="$emit('close')">×</button>
      <div class="modal-body">
        <div class="modal-image">
          <img :src="art.images.web.url" :alt="art.title" />
        </div>
        <div class="modal-text">
          <span class="accession">N° Registro: {{ art.accession_number }}</span>
          <h2>{{ art.title }}</h2>
          <p class="author-large">{{ art.creators[0]?.description }}</p>
          <div class="info-grid">
            <div class="info-item">
              <strong>Técnica</strong>
              <p>{{ art.technique || 'Óleo / Técnica mixta' }}</p>
            </div>
            <div class="info-item">
              <strong>Fecha</strong>
              <p>{{ art.creation_date || 'Sin fecha' }}</p>
            </div>
          </div>
          <div class="description-box">
            <strong>Contexto Histórico</strong>
            <p>{{ art.wall_description || 'Esta pieza es una parte fundamental de la colección.' }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.modal-overlay {
  position: fixed; top: 0; left: 0; width: 100%; height: 100%;
  background: rgba(0,0,0,0.9); z-index: 100;
  display: flex; align-items: center; justify-content: center; padding: 20px;
}
.modal-content { background: #111; width: 100%; max-width: 1100px; max-height: 90vh; overflow-y: auto; position: relative; border: 1px solid #333; color: white; }
.modal-body { display: grid; grid-template-columns: 1fr 1fr; gap: 40px; padding: 50px; }
.modal-image img { width: 100%; border-radius: 2px; }
.modal-text h2 { font-size: 2.5rem; margin: 15px 0; }
.info-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 30px; margin-top: 20px; }
.close-btn { position: absolute; top: 20px; right: 25px; background: none; border: none; color: white; font-size: 2.5rem; cursor: pointer; }
@media (max-width: 900px) { .modal-body { grid-template-columns: 1fr; } }
</style>