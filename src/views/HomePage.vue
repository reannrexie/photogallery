```vue
<template>
  <ion-page>

    <!-- HEADER -->
    <ion-header>
      <ion-toolbar>
        <ion-title>Event Planner</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content>

      <div class="container">

        <!-- HERO / HEADER CARD -->
        <div class="header-card">

          <div class="header-content">

            <div class="header-icon">
              📅
            </div>

            <div>
              <p class="welcome-text">
                ORGANIZE YOUR SCHEDULE
              </p>

              <h1>Event Planner</h1>

              <p class="subtitle">
                Manage your events easily and keep everything organized.
              </p>
            </div>

          </div>

          <ion-button
            class="add-button"
            @click="openForm"
          >
            <span class="plus-icon">+</span>
            Add New Event
          </ion-button>

        </div>


        <!-- EVENT FORM -->
        <EventFormComponent
          v-if="showForm"
          :editing-event="editingEvent"
          @save="saveEvent"
          @cancel="cancelForm"
        />


        <!-- EVENT LIST -->
        <EventListComponent
          :events="events"
          @edit="editEvent"
          @delete="deleteEvent"
        />

      </div>

    </ion-content>

  </ion-page>
</template>


<script setup lang="ts">

import { ref } from 'vue';

import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
  IonButton
} from '@ionic/vue';

import EventFormComponent from '@/components/EventFormComponent.vue';
import EventListComponent from '@/components/EventListComponent.vue';


interface EventItem {
  id: number;
  name: string;
  date: string;
  time: string;
  venue: string;
  description: string;
  status: string;
}


const events = ref<EventItem[]>([]);

const showForm = ref(false);

const editingEvent = ref<EventItem | null>(null);


const openForm = () => {

  editingEvent.value = null;

  showForm.value = true;

};


const cancelForm = () => {

  editingEvent.value = null;

  showForm.value = false;

};


const saveEvent = (event: EventItem) => {

  if (editingEvent.value) {

    const index = events.value.findIndex(
      item => item.id === editingEvent.value?.id
    );

    if (index !== -1) {

      events.value[index] = {
        ...event,
        id: editingEvent.value.id
      };

    }

  } else {

    events.value.push({
      ...event,
      id: Date.now()
    });

  }

  editingEvent.value = null;

  showForm.value = false;

  alert('Event saved successfully!');

};


const editEvent = (event: EventItem) => {

  editingEvent.value = {
    ...event
  };

  showForm.value = true;

};


const deleteEvent = (id: number) => {

  events.value = events.value.filter(
    event => event.id !== id
  );

};

</script>


<style scoped>

/* =========================================
   TOOLBAR
========================================= */

ion-toolbar {

  --background: #3e6985;

  --color: white;

  --min-height: 58px;

}


ion-title {

  font-size: 18px;

  font-weight: 700;

  letter-spacing: 0.2px;

}


/* =========================================
   PAGE
========================================= */

ion-content {

  --background: #f4f8fb;

}


.container {

  width: 100%;

  max-width: 1100px;

  margin: 0 auto;

  padding: 28px 18px 60px;

}


/* =========================================
   HEADER CARD
========================================= */

.header-card {

  background: white;

  border: 1px solid #d8e5ec;

  border-radius: 20px;

  padding: 24px;

  display: flex;

  justify-content: space-between;

  align-items: center;

  gap: 20px;

  margin-bottom: 24px;

  box-shadow:
    0 6px 20px rgba(13, 39, 61, 0.05);

}


.header-content {

  display: flex;

  align-items: center;

  gap: 15px;

}


.header-icon {

  width: 58px;

  height: 58px;

  flex-shrink: 0;

  display: flex;

  align-items: center;

  justify-content: center;

  background: #e8f2f7;

  border: 1px solid #d2e3ed;

  border-radius: 15px;

  font-size: 27px;

}


.welcome-text {

  margin: 0 0 5px;

  color: #7290a3;

  font-size: 10px;

  font-weight: 700;

  letter-spacing: 1px;

}


.header-card h1 {

  margin: 0;

  color: #0d273d;

  font-size: 28px;

  font-weight: 750;

}


.subtitle {

  margin: 5px 0 0;

  color: #718494;

  font-size: 13px;

  line-height: 1.4;

}


/* =========================================
   ADD BUTTON
========================================= */

.add-button {

  --background: #3e6985;

  --background-hover: #315a74;

  --border-radius: 11px;

  --box-shadow: none;

  --padding-start: 17px;

  --padding-end: 17px;

  height: 43px;

  margin: 0;

  font-size: 13px;

  font-weight: 650;

  white-space: nowrap;

}


.plus-icon {

  font-size: 21px;

  line-height: 1;

  margin-right: 6px;

}


/* =========================================
   FORM SPACING
========================================= */

EventFormComponent {

  display: block;

  margin-bottom: 24px;

}


/* =========================================
   MOBILE
========================================= */

@media (max-width: 700px) {

  .container {

    padding: 20px 14px 45px;

  }


  .header-card {

    padding: 20px;

    flex-direction: column;

    align-items: stretch;

  }


  .header-content {

    align-items: flex-start;

  }


  .add-button {

    width: 100%;

  }

}


@media (max-width: 450px) {

  .header-card {

    border-radius: 16px;

  }


  .header-icon {

    width: 48px;

    height: 48px;

    font-size: 22px;

  }


  .header-card h1 {

    font-size: 23px;

  }


  .subtitle {

    font-size: 12px;

  }

}

</style>
```
