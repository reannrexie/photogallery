```vue
<template>
  <ion-card class="gallery-card">
    <ion-card-header>
      <div class="gallery-icon-wrapper">
        <ion-icon :icon="imagesIcon" />
      </div>

      <ion-card-title>Photo Gallery</ion-card-title>

      <p class="gallery-subtitle">
        Your captured photos
      </p>
    </ion-card-header>

    <ion-card-content>
      <!-- Empty Gallery -->
      <div v-if="photos.length === 0" class="empty-gallery">
        <div class="empty-icon">
          <ion-icon :icon="imageOutline" />
        </div>

        <h3>No pictures yet</h3>

        <p>
          Take a picture using the camera
          and it will appear here.
        </p>
      </div>

      <!-- Photos -->
      <ion-grid v-else class="photo-grid">
        <ion-row>
          <ion-col
            v-for="(photo, index) in photos"
            :key="index"
            size="6"
            size-md="4"
          >
            <div class="photo-item">
              <ion-img
                :src="photo"
                class="gallery-image"
              />

              <div class="photo-number">
                {{ index + 1 }}
              </div>
            </div>
          </ion-col>
        </ion-row>
      </ion-grid>
    </ion-card-content>
  </ion-card>
</template>

<script setup lang="ts">
import {
  IonCard,
  IonCardHeader,
  IonCardTitle,
  IonCardContent,
  IonGrid,
  IonRow,
  IonCol,
  IonImg,
  IonIcon,
} from "@ionic/vue";

import {
  imagesOutline as imagesIcon,
  imageOutline,
} from "ionicons/icons";

defineProps<{
  photos: string[];
}>();
</script>

<style scoped>
/* Main Gallery Card */
.gallery-card {
  margin: 0 0 24px;

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

/* Gallery Icon */
.gallery-icon-wrapper {
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

.gallery-icon-wrapper ion-icon {
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
.gallery-subtitle {
  margin: 7px 0 0;

  color: #718494;
  font-size: 13px;
  line-height: 1.5;
}

/* Content */
ion-card-content {
  padding: 16px 20px 26px;
}

/* Empty Gallery */
.empty-gallery {
  padding: 38px 20px;
  text-align: center;
}

.empty-icon {
  width: 70px;
  height: 70px;
  margin: 0 auto 16px;

  display: flex;
  align-items: center;
  justify-content: center;

  background: #f0f5f8;
  border: 1px solid #dce7ee;
  border-radius: 20px;
}

.empty-icon ion-icon {
  font-size: 34px;
  color: #8aa7bc;
}

.empty-gallery h3 {
  margin: 0 0 7px;

  color: #0d273d;
  font-size: 18px;
  font-weight: 700;
}

.empty-gallery p {
  max-width: 290px;
  margin: 0 auto;

  color: #718494;
  font-size: 13px;
  line-height: 1.6;
}

/* Photo Grid */
.photo-grid {
  padding: 0;
}

.photo-grid ion-row {
  margin: -6px;
}

.photo-grid ion-col {
  padding: 6px;
}

/* Photo */
.photo-item {
  position: relative;

  overflow: hidden;

  background: #f5f8fa;

  border: 1px solid #d7e3ec;
  border-radius: 15px;

  box-shadow: 0 4px 12px rgba(62, 105, 133, 0.08);
}

/* Image */
.gallery-image {
  width: 100%;
  height: 180px;

  display: block;
}

.gallery-image::part(image) {
  object-fit: cover;
}

/* Photo Number */
.photo-number {
  position: absolute;

  top: 8px;
  right: 8px;

  min-width: 28px;
  height: 28px;

  padding: 0 7px;

  display: flex;
  align-items: center;
  justify-content: center;

  background: rgba(13, 39, 61, 0.80);
  color: #ffffff;

  border-radius: 9px;

  font-size: 12px;
  font-weight: 600;
}

/* Mobile */
@media (max-width: 576px) {
  ion-card-header {
    padding: 22px 16px 12px;
  }

  ion-card-content {
    padding: 14px 16px 22px;
  }

  .gallery-icon-wrapper {
    width: 54px;
    height: 54px;
    border-radius: 15px;
  }

  .gallery-icon-wrapper ion-icon {
    font-size: 26px;
  }

  ion-card-title {
    font-size: 19px;
  }

  .gallery-subtitle {
    font-size: 12px;
  }

  .gallery-image {
    height: 150px;
  }

  .empty-gallery {
    padding: 30px 15px;
  }
}
</style>
```

This will give you a **white + soft blue + navy** look, with rounded photo cards and a cleaner empty-gallery state. It also stays consistent with the aesthetic Camera component.
