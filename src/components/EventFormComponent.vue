```vue
<template>
  <div class="form-card">

    <!-- FORM HEADER -->
    <div class="form-header">
      <div class="form-icon">
        ✨
      </div>

      <div>
        <p class="form-label">EVENT DETAILS</p>
        <h2>
          {{ editingEvent ? 'Edit Event' : 'Add New Event' }}
        </h2>
        <p class="form-subtitle">
          {{ editingEvent
            ? 'Update the details of your event.'
            : 'Fill in the details below to create a new event.'
          }}
        </p>
      </div>
    </div>

    <!-- FORM -->
    <div class="form-content">

      <!-- EVENT NAME -->
      <div class="field full">
        <ion-label>Event Name</ion-label>

        <ion-input
          v-model="form.name"
          placeholder="Enter event name"
          fill="outline"
        />
      </div>

      <!-- DATE + TIME -->
      <div class="form-grid">

        <div class="field">
          <ion-label>Date</ion-label>

          <ion-input
            v-model="form.date"
            type="date"
            fill="outline"
          />
        </div>

        <div class="field">
          <ion-label>Time</ion-label>

          <ion-input
            v-model="form.time"
            type="time"
            fill="outline"
          />
        </div>

      </div>

      <!-- VENUE -->
      <div class="field full">
        <ion-label>Venue</ion-label>

        <ion-input
          v-model="form.venue"
          placeholder="Enter event venue"
          fill="outline"
        />
      </div>

      <!-- DESCRIPTION -->
      <div class="field full">
        <ion-label>Description</ion-label>

        <ion-textarea
          v-model="form.description"
          placeholder="Enter event description"
          fill="outline"
          :auto-grow="true"
          :rows="4"
        />
      </div>

      <!-- STATUS -->
      <div class="field full">
        <ion-label>Status</ion-label>

        <ion-select
          v-model="form.status"
          placeholder="Select status"
          fill="outline"
        >
          <ion-select-option value="Planned">
            Planned
          </ion-select-option>

          <ion-select-option value="Ongoing">
            Ongoing
          </ion-select-option>

          <ion-select-option value="Completed">
            Completed
          </ion-select-option>

          <ion-select-option value="Cancelled">
            Cancelled
          </ion-select-option>
        </ion-select>
      </div>

    </div>

    <!-- BUTTONS -->
    <div class="form-buttons">

      <ion-button
        class="cancel-button"
        fill="outline"
        @click="cancelForm"
      >
        Cancel
      </ion-button>

      <ion-button
        class="save-button"
        @click="saveEvent"
      >
        {{ editingEvent ? 'Update Event' : 'Save Event' }}
      </ion-button>

    </div>

  </div>
</template>

<script setup lang="ts">
import { reactive, watch } from 'vue';

import {
  IonButton,
  IonLabel,
  IonInput,
  IonTextarea,
  IonSelect,
  IonSelectOption
} from '@ionic/vue';

interface EventItem {
  id: number;
  name: string;
  date: string;
  time: string;
  venue: string;
  description: string;
  status: string;
}

const props = defineProps<{
  editingEvent: EventItem | null;
}>();

const emit = defineEmits<{
  (e: 'save', event: EventItem): void;
  (e: 'cancel'): void;
}>();

const form = reactive<EventItem>({
  id: 0,
  name: '',
  date: '',
  time: '',
  venue: '',
  description: '',
  status: 'Planned'
});

const clearForm = () => {
  form.id = 0;
  form.name = '';
  form.date = '';
  form.time = '';
  form.venue = '';
  form.description = '';
  form.status = 'Planned';
};

const loadEvent = () => {
  if (props.editingEvent) {
    form.id = props.editingEvent.id;
    form.name = props.editingEvent.name;
    form.date = props.editingEvent.date;
    form.time = props.editingEvent.time;
    form.venue = props.editingEvent.venue;
    form.description = props.editingEvent.description;
    form.status = props.editingEvent.status;
  } else {
    clearForm();
  }
};

watch(
  () => props.editingEvent,
  () => {
    loadEvent();
  },
  {
    immediate: true
  }
);

const saveEvent = () => {

  if (
    form.name.trim() === '' ||
    form.date === '' ||
    form.time === '' ||
    form.venue.trim() === ''
  ) {
    alert(
      'Please fill in Event Name, Date, Time, and Venue.'
    );

    return;
  }

  emit('save', {
    id: form.id,
    name: form.name,
    date: form.date,
    time: form.time,
    venue: form.venue,
    description: form.description,
    status: form.status
  });

  clearForm();
};

const cancelForm = () => {
  clearForm();
  emit('cancel');
};
</script>

<style scoped>

.form-card {
  background: #ffffff;
  border: 1px solid #d8e5ec;
  border-radius: 20px;
  padding: 26px;
  margin-bottom: 24px;

  box-shadow:
    0 6px 20px rgba(13, 39, 61, 0.05);
}

/* FORM HEADER */

.form-header {
  display: flex;
  align-items: center;
  gap: 15px;

  padding-bottom: 20px;
  margin-bottom: 22px;

  border-bottom: 1px solid #e4edf2;
}

.form-icon {
  width: 52px;
  height: 52px;

  display: flex;
  align-items: center;
  justify-content: center;

  background: #e8f2f7;
  border: 1px solid #d2e3ed;

  border-radius: 14px;

  font-size: 23px;
}

.form-label {
  margin: 0 0 4px;

  color: #7290a3;

  font-size: 10px;
  font-weight: 700;

  letter-spacing: 1px;
}

.form-header h2 {
  margin: 0;

  color: #0d273d;

  font-size: 23px;
  font-weight: 750;
}

.form-subtitle {
  margin: 4px 0 0;

  color: #718494;

  font-size: 12px;
}

/* FORM CONTENT */

.form-content {
  display: flex;
  flex-direction: column;
  gap: 18px;
}

.form-grid {
  display: grid;

  grid-template-columns: 1fr 1fr;

  gap: 16px;
}

.field {
  display: flex;
  flex-direction: column;
  gap: 7px;
}

.field ion-label {
  color: #3e6985;

  font-size: 12px;
  font-weight: 700;
}

ion-input,
ion-textarea,
ion-select {
  --background: #ffffff;

  --border-color: #cbdbe5;
  --border-width: 1px;
  --border-style: solid;

  --border-radius: 10px;

  --padding-start: 12px;
  --padding-end: 12px;

  color: #0d273d;

  font-size: 13px;
}

/* FOCUS */

ion-input.ion-focused,
ion-textarea.ion-focused,
ion-select.ion-focused {
  --border-color: #3e6985;
}

/* BUTTONS */

.form-buttons {
  display: flex;
  justify-content: flex-end;

  gap: 10px;

  margin-top: 24px;
  padding-top: 20px;

  border-top: 1px solid #e4edf2;
}

.form-buttons ion-button {
  height: 42px;

  margin: 0;

  --border-radius: 10px;

  --box-shadow: none;

  font-size: 13px;
  font-weight: 650;
}

.cancel-button {
  --color: #3e6985;

  --border-color: #b9cedb;

  --border-width: 1px;
}

.save-button {
  --background: #3e6985;

  --background-hover: #315a74;

  --color: #ffffff;
}

/* MOBILE */

@media (max-width: 650px) {

  .form-card {
    padding: 20px;

    border-radius: 17px;
  }

  .form-grid {
    grid-template-columns: 1fr;
  }

  .form-header {
    align-items: flex-start;
  }

  .form-header h2 {
    font-size: 20px;
  }

  .form-buttons {
    flex-direction: column-reverse;
  }

  .form-buttons ion-button {
    width: 100%;
  }

}

</style>
```

### Ito ang magiging flow:

**Homepage → ****+ Add New Event**** → Form lalabas → Save Event → My Events**

At kapag pinindot ang **Edit**, parehong form ang gagamitin pero magiging **“Edit Event”** at **“Update Event”** ang buttons/text.

**Files mo ngayon:**

```text
src/
├── components/
│   ├── EventFormComponent.vue    ← ITO YUNG FORM
│   └── EventListComponent.vue    ← MY EVENTS
│
└── views/
    └── HomePage.vue              ← HOMEPAGE
```
