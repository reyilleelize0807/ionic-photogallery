<template>
  <ion-card class="camera-card">
    <ion-card-content>
      <h2 class="camera-title">Camera</h2>

      <ion-button
        expand="block"
        class="take-picture-button"
        @click="takePicture"
      >
        <ion-icon
          slot="start"
          :icon="cameraIcon"
        />
        TAKE PICTURE
      </ion-button>

      <ion-text
        v-if="errorMessage"
        color="danger"
      >
        <p class="error-message">
          {{ errorMessage }}
        </p>
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
      emit('photoCaptured', capturedPhoto.webPath)
    }
  } catch (error) {
    console.error('Camera error:', error)
    errorMessage.value = 'Unable to capture photo. Please try again.'
  }
}
</script>

<style scoped>
.camera-card {
  margin: 0 0 42px 0;

  --background: #f1f2ff;

  border-radius: 28px;

  box-shadow: none;
}

.camera-card ion-card-content {
  padding: 34px 32px 38px;
}

.camera-title {
  margin: 0 0 22px;

  font-size: 32px;
  font-weight: 700;

  color: #202846;
}

/* TAKE PICTURE BUTTON */
.take-picture-button {
  --background: linear-gradient(
    90deg,
    #4f46e5,
    #6366f1
  );

  --background-hover: #4f46e5;

  --border-radius: 50px;

  --box-shadow: none;

  height: 64px;

  font-size: 20px;
  font-weight: 700;

  letter-spacing: 0.3px;
}

.take-picture-button ion-icon {
  font-size: 30px;
}

.error-message {
  margin-top: 15px;
  text-align: center;
  font-size: 14px;
}

/* MOBILE */
@media (max-width: 600px) {
  .camera-card {
    border-radius: 22px;
    margin-bottom: 32px;
  }

  .camera-card ion-card-content {
    padding: 25px 20px 28px;
  }

  .camera-title {
    font-size: 26px;
    margin-bottom: 18px;
  }

  .take-picture-button {
    height: 56px;
    font-size: 16px;
  }

  .take-picture-button ion-icon {
    font-size: 25px;
  }
}
</style>