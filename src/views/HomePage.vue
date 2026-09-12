<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>My Photo Gallery</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content>

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
  IonHeader,
  IonPage,
  IonTitle,
  IonToolbar,
} from '@ionic/vue'

import CameraComponent from '@/components/CameraComponent.vue'
import PhotoGalleryComponent from '@/components/PhotoGalleryComponent.vue'

const photos = ref<string[]>([])

const addPhoto = (photo: string) => {
  photos.value.unshift(photo)
}
</script>