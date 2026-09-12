<template>
  <ion-card class="camera-card">
    <ion-card-header>
      <ion-card-title>Camera</ion-card-title>
    </ion-card-header>

    <ion-card-content>
      <ion-button
        expand="block"
        class="take-photo-button"
        @click="takePicture"
      >
        <ion-icon slot="start" :icon="cameraIcon" />
        TAKE PICTURE
      </ion-button>

      <ion-text v-if="errorMessage" color="danger">
        <p class="error-message">{{ errorMessage }}</p>
      </ion-text>
    </ion-card-content>
  </ion-card>
</template>

<script setup lang="ts">
import { ref } from 'vue'

import {
  IonButton,
  IonCard,
  IonCardContent,
  IonCardHeader,
  IonCardTitle,
  IonIcon,
  IonText,
} from '@ionic/vue'

import { camera as cameraIcon } from 'ionicons/icons'

import {
  Camera,
  CameraResultType,
  CameraSource,
} from '@capacitor/camera'

const errorMessage = ref('')

const emit = defineEmits<{
  (event: 'photoCaptured', photo: string): void
}>()

const takePicture = async () => {
  errorMessage.value = ''

  try {
    const capturedPhoto = await Camera.getPhoto({
      quality: 90,
      allowEditing: false,
      resultType: CameraResultType.Uri,
      source: CameraSource.Camera,
    })

    if (capturedPhoto.webPath) {
      // Send photo to parent
      emit('photoCaptured', capturedPhoto.webPath)
    }
  } catch (error) {
    console.error('Camera error:', error)
    errorMessage.value = 'Unable to capture photo.'
  }
}
</script>

<style scoped>
.camera-card {
  margin: 16px;
  border-radius: 5px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.15);
}

ion-card-title {
  font-size: 20px;
  font-weight: 500;
}

.take-photo-button {
  --background: #0d5be1;
  --border-radius: 4px;

  font-weight: 500;
}

.error-message {
  text-align: center;
  margin-top: 12px;
}
</style>