<template>
  <div class="gallery-container">

    <!-- Gallery Header -->
    <div class="gallery-header">
      <div>
        <h2>Photo Gallery</h2>
        <p>
          {{ photos.length }}
          photo{{ photos.length !== 1 ? 's' : '' }}
        </p>
      </div>
    </div>

    <!-- Empty State -->
    <div
      v-if="photos.length === 0"
      class="empty-gallery"
    >
      <div class="empty-icon">
        <ion-icon :icon="imagesIcon" />
      </div>

      <h3>No Photos Yet</h3>

      <p>
        Take a picture to add photos to your gallery.
      </p>
    </div>

    <!-- Photo Gallery -->
    <ion-grid
      v-else
      class="photo-grid"
    >
      <ion-row>
        <ion-col
          v-for="(photo, index) in photos"
          :key="index"
          size="6"
          size-md="4"
          size-lg="3"
        >
          <div
            class="photo-card"
            @click="openPhoto(photo)"
          >
            <img
              :src="photo"
              :alt="`Photo ${index + 1}`"
            />

            <div class="photo-overlay">
              <ion-icon :icon="expandIcon" />
            </div>
          </div>
        </ion-col>
      </ion-row>
    </ion-grid>

    <!-- Photo Preview Modal -->
    <ion-modal
      :is-open="selectedPhoto !== null"
      @didDismiss="closePhoto"
    >
      <ion-header>
        <ion-toolbar>
          <ion-title>Photo Preview</ion-title>

          <ion-buttons slot="end">
            <ion-button @click="closePhoto">
              Close
            </ion-button>
          </ion-buttons>
        </ion-toolbar>
      </ion-header>

      <ion-content class="preview-content">
        <img
          v-if="selectedPhoto"
          :src="selectedPhoto"
          class="preview-image"
          alt="Selected photo"
        />
      </ion-content>
    </ion-modal>

  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

import {
  IonButton,
  IonButtons,
  IonCol,
  IonContent,
  IonGrid,
  IonHeader,
  IonIcon,
  IonModal,
  IonRow,
  IonTitle,
  IonToolbar,
} from '@ionic/vue'

import {
  images as imagesIcon,
  expand as expandIcon,
} from 'ionicons/icons'

defineProps<{
  photos: string[]
}>()

const selectedPhoto = ref<string | null>(null)

const openPhoto = (photo: string) => {
  selectedPhoto.value = photo
}

const closePhoto = () => {
  selectedPhoto.value = null
}
</script>

<style scoped>
.gallery-container {
  width: 100%;
  padding: 20px;
}

/* Header */
.gallery-header {
  margin-bottom: 15px;
}

.gallery-header h2 {
  margin: 0;

  font-size: 24px;
  font-weight: 700;

  color: #1f2937;
}

.gallery-header p {
  margin: 5px 0 0;

  font-size: 14px;
  color: #6b7280;
}

/* Empty Gallery */
.empty-gallery {
  padding: 50px 20px;

  text-align: center;

  border: 2px dashed #d9d9e3;
  border-radius: 20px;

  background: #faf9fc;
}

.empty-icon {
  width: 80px;
  height: 80px;

  margin: 0 auto 20px;

  display: flex;
  align-items: center;
  justify-content: center;

  border-radius: 50%;

  background: #eee6f5;
}

.empty-icon ion-icon {
  font-size: 40px;
  color: #4b0082;
}

.empty-gallery h3 {
  margin: 0 0 8px;

  font-size: 20px;
  color: #333;
}

.empty-gallery p {
  margin: 0;

  font-size: 14px;
  color: #777;
}

/* Gallery Grid */
.photo-grid {
  padding: 0;
}

.photo-grid ion-col {
  padding: 5px;
}

/* Photo */
.photo-card {
  position: relative;

  width: 100%;
  aspect-ratio: 1 / 1;

  overflow: hidden;

  border-radius: 15px;

  background: #eee;

  cursor: pointer;

  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.08);

  transition: 0.2s ease;
}

.photo-card:hover {
  transform: translateY(-3px);
}

.photo-card img {
  width: 100%;
  height: 100%;

  display: block;

  object-fit: cover;
}

/* Hover Overlay */
.photo-overlay {
  position: absolute;

  inset: 0;

  display: flex;
  align-items: center;
  justify-content: center;

  background: rgba(0, 0, 0, 0);

  opacity: 0;

  transition: 0.2s ease;
}

.photo-overlay ion-icon {
  font-size: 30px;
  color: white;
}

.photo-card:hover .photo-overlay {
  background: rgba(0, 0, 0, 0.35);
  opacity: 1;
}

/* Preview */
.preview-content {
  --background: #000;

  display: flex;
  align-items: center;
  justify-content: center;
}

.preview-image {
  width: 100%;
  height: 100%;

  object-fit: contain;
}

/* Mobile */
@media (max-width: 600px) {
  .gallery-container {
    padding: 15px;
  }

  .gallery-header h2 {
    font-size: 20px;
  }

  .photo-grid ion-col {
    padding: 4px;
  }

  .photo-card {
    border-radius: 12px;
  }
}
</style>