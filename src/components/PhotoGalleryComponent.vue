<template>
  <div class="gallery-container">

    <!-- Gallery Header -->
    <div class="gallery-header">
      <h2>Photo Gallery</h2>

      <p>
        {{ photos.length }}
        photo{{ photos.length !== 1 ? 's' : '' }}
      </p>
    </div>

    <!-- Empty Gallery -->
    <div
      v-if="photos.length === 0"
      class="empty-gallery"
    >
      <div class="empty-icon">
        <ion-icon :icon="imagesIcon" />
      </div>

      <h3>No Photos Yet</h3>

      <p>
        Take a picture to start your gallery.
      </p>
    </div>

    <!-- Photo Grid -->
    <ion-grid
      v-else
      class="photo-grid"
    >
      <ion-row>
        <ion-col
          v-for="(photo, index) in photos"
          :key="index"
          size="6"
          size-sm="6"
          size-md="6"
          size-lg="6"
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

    <!-- Full Photo Preview -->
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
}

/* HEADER */
.gallery-header {
  margin-bottom: 24px;
}

.gallery-header h2 {
  margin: 0;

  font-size: 32px;
  font-weight: 700;

  color: #17213f;
}

.gallery-header p {
  margin: 5px 0 0;

  font-size: 20px;

  color: #7d849b;
}

/* EMPTY STATE */
.empty-gallery {
  padding: 50px 20px;

  text-align: center;

  border: 2px dashed #d9dcef;
  border-radius: 20px;

  background: #fafaff;
}

.empty-icon {
  width: 75px;
  height: 75px;

  margin: 0 auto 15px;

  display: flex;
  align-items: center;
  justify-content: center;

  border-radius: 50%;

  background: #eef0ff;
}

.empty-icon ion-icon {
  font-size: 38px;
  color: #5b5fea;
}

.empty-gallery h3 {
  margin: 0 0 8px;

  font-size: 20px;
  color: #202846;
}

.empty-gallery p {
  margin: 0;

  color: #7d849b;
}

/* GRID */
.photo-grid {
  padding: 0;
  margin: 0 -6px;
}

.photo-grid ion-col {
  padding: 6px;
}

/* PHOTO */
.photo-card {
  position: relative;

  width: 100%;

  aspect-ratio: 1.45 / 1;

  overflow: hidden;

  border-radius: 20px;

  background: #eeeeee;

  cursor: pointer;

  transition:
    transform 0.2s ease,
    box-shadow 0.2s ease;
}

.photo-card:hover {
  transform: translateY(-3px);

  box-shadow:
    0 8px 20px rgba(30, 40, 80, 0.15);
}

.photo-card img {
  width: 100%;
  height: 100%;

  display: block;

  object-fit: cover;
}

/* IMAGE HOVER */
.photo-overlay {
  position: absolute;

  inset: 0;

  display: flex;
  align-items: center;
  justify-content: center;

  background: rgba(20, 25, 60, 0);

  opacity: 0;

  transition: 0.2s ease;
}

.photo-overlay ion-icon {
  font-size: 32px;
  color: white;
}

.photo-card:hover .photo-overlay {
  background: rgba(20, 25, 60, 0.35);
  opacity: 1;
}

/* MODAL */
.preview-content {
  --background: #000;
}

.preview-image {
  width: 100%;
  height: 100%;

  object-fit: contain;

  display: block;
}

/* TABLET */
@media (max-width: 768px) {
  .gallery-header h2 {
    font-size: 28px;
  }

  .gallery-header p {
    font-size: 17px;
  }

  .photo-card {
    border-radius: 16px;
  }
}

/* MOBILE */
@media (max-width: 480px) {
  .gallery-header h2 {
    font-size: 25px;
  }

  .gallery-header p {
    font-size: 16px;
  }

  .photo-grid {
    margin: 0 -4px;
  }

  .photo-grid ion-col {
    padding: 4px;
  }

  .photo-card {
    border-radius: 13px;
    aspect-ratio: 1 / 1;
  }
}
</style>