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
  margin: 12px 16px;
  border-radius: 20px;
  background: #f7f7ff;
  box-shadow: none;
  padding: 2px;
}

.camera-card ion-card-header {
  padding: 8px 16px 2px;
}

.camera-card ion-card-content {
  padding: 4px 16px 10px;
}

ion-card-title {
  font-size: 20px;
  font-weight: 600;
}

.take-photo-button {
  --background: #7c3aed;
  --background-hover: #6d28d9;
  --border-radius: 9999px;

  height: 44px;

  font-size: 14px;
  font-weight: 700;

  margin-top: 4px;
}

.take-photo-button ion-icon {
  font-size: 22px;
  margin-right: 8px;
}

.error-message {
  text-align: center;
  margin-top: 12px;
  font-size: 13px;
}
</style>