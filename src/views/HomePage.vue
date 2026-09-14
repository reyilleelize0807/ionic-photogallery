<template>
  <ion-page>

    <ion-content>

      <!-- Page Title -->
      <div class="page-title">
        My Photo Gallery
      </div>

      <!-- Camera -->
      <CameraComponent
        @photoCaptured="addPhoto"
      />

      <!-- Gallery -->
      <PhotoGalleryComponent
        :photos="photos"
      />

    </ion-content>

  </ion-page>
</template>

<script setup lang="ts">
import { ref } from 'vue'

import {
  IonContent,
  IonPage,
} from '@ionic/vue'

import CameraComponent from '@/components/CameraComponent.vue'
import PhotoGalleryComponent from '@/components/PhotoGalleryComponent.vue'

const photos = ref<string[]>([])

const addPhoto = (photo: string) => {
  photos.value.unshift(photo)
}
</script>

<style scoped>
ion-content {
  --background: var(--app-background);
}

.page-title {
  padding: calc(20px + env(safe-area-inset-top)) 16px 4px;

  font-size: 24px;
  font-weight: 700;

  color: var(--app-text);
}
</style>