```vue
<template>
  <ion-card class="camera-card">
    <ion-card-header>
      <div class="camera-icon-wrapper">
        <ion-icon :icon="cameraIcon" />
      </div>

      <ion-card-title>Camera</ion-card-title>
      <p class="camera-subtitle">
        Capture a photo for your gallery
      </p>
    </ion-card-header>

    <ion-card-content>
      <ion-button
        expand="block"
        class="take-photo-button"
        @click="takePicture"
      >
        <ion-icon slot="start" :icon="cameraIcon" />
        Take Picture
      </ion-button>

      <ion-text v-if="errorMessage" class="error-message">
        <p>{{ errorMessage }}</p>
      </ion-text>
    </ion-card-content>
  </ion-card>
</template>

<script setup lang="ts">
import {
  IonButton,
  IonCard,
  IonCardHeader,
  IonCardTitle,
  IonCardContent,
  IonIcon,
  IonText,
} from "@ionic/vue";

import { camera as cameraIcon } from "ionicons/icons";
import { Camera } from "@capacitor/camera";
import { ref } from "vue";

const errorMessage = ref("");

const emit = defineEmits<{
  (event: "photoCaptured", photo: string): void;
}>();

const takePicture = async () => {
  errorMessage.value = "";

  try {
    const photo = await Camera.takePhoto({
      quality: 90,
      saveToGallery: false,
    });

    if (photo.webPath) {
      emit("photoCaptured", photo.webPath);
    }
  } catch (error) {
    console.error(error);

    errorMessage.value =
      "Unable to capture photo. Please check your camera permissions.";
  }
};
</script>

<style scoped>
/* Main Card */
.camera-card {
  margin: 18px 0 24px;
  background: #ffffff;
  border: 1px solid #d7e3ec;
  border-radius: 20px;
  box-shadow: 0 6px 18px rgba(62, 105, 133, 0.10);
  overflow: hidden;
}

/* Header */
ion-card-header {
  text-align: center;
  padding: 26px 20px 14px;
}

/* Camera Icon */
.camera-icon-wrapper {
  width: 62px;
  height: 62px;
  margin: 0 auto 14px;

  display: flex;
  align-items: center;
  justify-content: center;

  background: #e8f1f7;
  border: 1px solid #d1e1eb;
  border-radius: 18px;
}

.camera-icon-wrapper ion-icon {
  font-size: 30px;
  color: #3e6985;
}

/* Title */
ion-card-title {
  color: #0d273d;
  font-size: 22px;
  font-weight: 700;
  letter-spacing: 0.2px;
}

/* Subtitle */
.camera-subtitle {
  margin: 7px 0 0;
  color: #718494;
  font-size: 13px;
  line-height: 1.5;
}

/* Content */
ion-card-content {
  padding: 16px 20px 26px;
}

/* Take Picture Button */
.take-photo-button {
  height: 50px;

  --background: #3e6985;
  --background-hover: #315a75;
  --background-activated: #294f68;
  --color: #ffffff;

  --border-radius: 13px;
  --box-shadow: 0 5px 12px rgba(62, 105, 133, 0.18);

  font-size: 15px;
  font-weight: 600;
  text-transform: none;
  letter-spacing: 0.2px;
}

.take-photo-button ion-icon {
  font-size: 20px;
}

/* Error Message */
.error-message {
  display: block;
  margin-top: 14px;
  padding: 11px 13px;

  background: #fff4f3;
  border: 1px solid #f1d2cf;
  border-radius: 11px;

  color: #b44a42;
  font-size: 13px;
  text-align: center;
}

.error-message p {
  margin: 0;
}

/* Mobile */
@media (max-width: 576px) {
  .camera-card {
    border-radius: 17px;
  }

  ion-card-header {
    padding: 22px 16px 12px;
  }

  .camera-icon-wrapper {
    width: 54px;
    height: 54px;
    border-radius: 15px;
  }

  .camera-icon-wrapper ion-icon {
    font-size: 26px;
  }

  ion-card-title {
    font-size: 19px;
  }

  .camera-subtitle {
    font-size: 12px;
  }

  ion-card-content {
    padding: 14px 16px 22px;
  }
}
</style>
```


