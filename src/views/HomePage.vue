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
  --background: #f3e8ff;
}

.page-title {
  padding: 20px 16px 4px;

  font-size: 24px;
  font-weight: 700;

  color: #000000;
}
</style>