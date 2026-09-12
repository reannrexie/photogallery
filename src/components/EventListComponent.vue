```vue
<template>

  <div class="events-section">

    <!-- SECTION HEADER -->
    <div class="section-header">

      <div class="section-title">
        <div class="title-icon">
          📅
        </div>

        <div>
          <h2>My Events</h2>
          <p>Keep track of all your upcoming events</p>
        </div>
      </div>

      <div class="event-count">
        {{ events.length }} Event<span v-if="events.length !== 1">s</span>
      </div>

    </div>


    <!-- EMPTY STATE -->
    <div
      v-if="events.length === 0"
      class="empty-state"
    >

      <div class="empty-icon">
        📅
      </div>

      <h3>No Events Yet</h3>

      <p>
        Your events will appear here after you add one.
      </p>

    </div>


    <!-- EVENT LIST -->
    <div
      v-else
      class="event-list"
    >

      <div
        v-for="event in events"
        :key="event.id"
        class="event-card"
      >

        <!-- CARD TOP -->
        <div class="card-top">

          <div class="event-title-wrapper">

            <div class="calendar-icon">
              📅
            </div>

            <div>
              <h3>{{ event.name }}</h3>

              <span
                class="status"
                :class="event.status.toLowerCase()"
              >
                {{ event.status }}
              </span>
            </div>

          </div>

          <!-- ACTIONS -->
          <div class="actions">

            <button
              class="edit-btn"
              @click="editEvent(event)"
            >
              ✏️
              <span>Edit</span>
            </button>

            <button
              class="delete-btn"
              @click="deleteEvent(event.id)"
            >
              🗑️
              <span>Delete</span>
            </button>

          </div>

        </div>


        <!-- EVENT DETAILS -->
        <div class="event-details">

          <div class="detail-item">

            <div class="detail-icon">
              📆
            </div>

            <div>
              <span>Date</span>
              <strong>{{ event.date }}</strong>
            </div>

          </div>


          <div class="detail-item">

            <div class="detail-icon">
              🕐
            </div>

            <div>
              <span>Time</span>
              <strong>{{ event.time }}</strong>
            </div>

          </div>


          <div class="detail-item">

            <div class="detail-icon">
              📍
            </div>

            <div>
              <span>Venue</span>
              <strong>{{ event.venue }}</strong>
            </div>

          </div>

        </div>


        <!-- DESCRIPTION -->
        <div class="description-box">

          <span>Description</span>

          <p>
            {{ event.description || 'No description provided.' }}
          </p>

        </div>

      </div>

    </div>

  </div>

</template>


<script setup lang="ts">

import { IonButton } from '@ionic/vue';

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
  events: EventItem[];
}>();


const emit = defineEmits<{
  (e: 'edit', event: EventItem): void;

  (e: 'delete', id: number): void;
}>();


const editEvent = (event: EventItem) => {

  emit('edit', event);

};


const deleteEvent = (id: number) => {

  const confirmDelete = confirm(
    'Are you sure you want to delete this event?'
  );

  if (!confirmDelete) {
    return;
  }

  emit('delete', id);

};

</script>


<style scoped>

/* =========================================
   MAIN SECTION
========================================= */

.events-section {
  width: 100%;
}


/* =========================================
   SECTION HEADER
========================================= */

.section-header {

  display: flex;

  justify-content: space-between;

  align-items: center;

  margin-bottom: 18px;

  gap: 15px;

}


.section-title {

  display: flex;

  align-items: center;

  gap: 12px;

}


.title-icon {

  width: 46px;

  height: 46px;

  display: flex;

  align-items: center;

  justify-content: center;

  background: #e8f2f7;

  border: 1px solid #d3e4ee;

  border-radius: 12px;

  font-size: 21px;

}


.section-title h2 {

  margin: 0;

  color: #0d273d;

  font-size: 23px;

  font-weight: 700;

}


.section-title p {

  margin: 4px 0 0;

  color: #718494;

  font-size: 13px;

}


.event-count {

  background: #f0f6f9;

  border: 1px solid #d7e5ed;

  color: #3e6985;

  padding: 8px 13px;

  border-radius: 20px;

  font-size: 12px;

  font-weight: 700;

}


/* =========================================
   EMPTY STATE
========================================= */

.empty-state {

  background: #ffffff;

  border: 1px solid #d9e5ec;

  border-radius: 18px;

  padding: 55px 25px;

  text-align: center;

  box-shadow: 0 5px 20px rgba(13, 39, 61, 0.05);

}


.empty-icon {

  width: 65px;

  height: 65px;

  margin: 0 auto 15px;

  display: flex;

  align-items: center;

  justify-content: center;

  background: #eaf3f8;

  border-radius: 18px;

  font-size: 28px;

}


.empty-state h3 {

  margin: 0;

  color: #0d273d;

  font-size: 19px;

}


.empty-state p {

  margin: 7px 0 0;

  color: #718494;

  font-size: 14px;

}


/* =========================================
   EVENT LIST
========================================= */

.event-list {

  display: flex;

  flex-direction: column;

  gap: 15px;

}


/* =========================================
   EVENT CARD
========================================= */

.event-card {

  background: #ffffff;

  border: 1px solid #d8e4eb;

  border-radius: 18px;

  padding: 20px;

  box-shadow:
    0 5px 18px rgba(13, 39, 61, 0.05);

  transition:
    transform 0.2s ease,
    box-shadow 0.2s ease,
    border-color 0.2s ease;

}


.event-card:hover {

  transform: translateY(-2px);

  border-color: #b9cfdd;

  box-shadow:
    0 9px 25px rgba(13, 39, 61, 0.08);

}


/* =========================================
   CARD TOP
========================================= */

.card-top {

  display: flex;

  justify-content: space-between;

  align-items: flex-start;

  gap: 15px;

}


.event-title-wrapper {

  display: flex;

  align-items: center;

  gap: 12px;

  min-width: 0;

}


.calendar-icon {

  width: 45px;

  height: 45px;

  flex-shrink: 0;

  display: flex;

  align-items: center;

  justify-content: center;

  background: #edf5f9;

  border: 1px solid #d8e7ef;

  border-radius: 12px;

  font-size: 19px;

}


.event-title-wrapper h3 {

  margin: 0 0 7px;

  color: #0d273d;

  font-size: 18px;

  font-weight: 700;

  word-break: break-word;

}


/* =========================================
   STATUS
========================================= */

.status {

  display: inline-flex;

  align-items: center;

  padding: 5px 10px;

  border-radius: 20px;

  background: #e8f1f6;

  color: #3e6985;

  font-size: 11px;

  font-weight: 700;

}


.status::before {

  content: '';

  width: 6px;

  height: 6px;

  border-radius: 50%;

  background: currentColor;

  margin-right: 6px;

}


.status.ongoing {

  background: #e7f4ed;

  color: #28704c;

}


.status.completed {

  background: #e9eef7;

  color: #3f5f92;

}


.status.cancelled {

  background: #f8e8e8;

  color: #a04444;

}


/* =========================================
   ACTION BUTTONS
========================================= */

.actions {

  display: flex;

  gap: 7px;

  flex-shrink: 0;

}


.actions button {

  border: 1px solid;

  border-radius: 9px;

  padding: 8px 11px;

  background: white;

  font-family: inherit;

  font-size: 12px;

  font-weight: 600;

  cursor: pointer;

  transition: 0.2s ease;

}


.edit-btn {

  color: #3e6985;

  border-color: #cbdde7;

}


.edit-btn:hover {

  background: #edf5f9;

  border-color: #a9c4d3;

}


.delete-btn {

  color: #a04444;

  border-color: #edd0d0;

}


.delete-btn:hover {

  background: #fbefef;

  border-color: #dfb6b6;

}


/* =========================================
   EVENT DETAILS
========================================= */

.event-details {

  display: grid;

  grid-template-columns:
    repeat(3, 1fr);

  gap: 10px;

  margin-top: 18px;

}


.detail-item {

  display: flex;

  align-items: center;

  gap: 10px;

  padding: 12px;

  background: #f8fafc;

  border: 1px solid #edf1f4;

  border-radius: 11px;

}


.detail-icon {

  width: 34px;

  height: 34px;

  flex-shrink: 0;

  display: flex;

  align-items: center;

  justify-content: center;

  background: #eaf3f8;

  border-radius: 9px;

  font-size: 15px;

}


.detail-item span {

  display: block;

  color: #7a8d9b;

  font-size: 10px;

  font-weight: 600;

  text-transform: uppercase;

  letter-spacing: 0.4px;

}


.detail-item strong {

  display: block;

  margin-top: 3px;

  color: #18364c;

  font-size: 13px;

  font-weight: 600;

  word-break: break-word;

}


/* =========================================
   DESCRIPTION
========================================= */

.description-box {

  margin-top: 10px;

  padding: 13px;

  background: #f8fafc;

  border: 1px solid #edf1f4;

  border-radius: 11px;

}


.description-box span {

  color: #7a8d9b;

  font-size: 10px;

  font-weight: 600;

  text-transform: uppercase;

  letter-spacing: 0.4px;

}


.description-box p {

  margin: 5px 0 0;

  color: #425b6d;

  font-size: 13px;

  line-height: 1.5;

}


/* =========================================
   MOBILE
========================================= */

@media (max-width: 700px) {

  .section-header {

    align-items: flex-start;

  }


  .event-count {

    white-space: nowrap;

  }


  .card-top {

    flex-direction: column;

  }


  .actions {

    width: 100%;

  }


  .actions button {

    flex: 1;

  }


  .event-details {

    grid-template-columns: 1fr;

  }

}


@media (max-width: 450px) {

  .section-title p {

    display: none;

  }


  .section-title h2 {

    font-size: 20px;

  }


  .title-icon {

    width: 40px;

    height: 40px;

  }


  .event-card {

    padding: 15px;

  }


  .event-title-wrapper h3 {

    font-size: 16px;

  }

}

</style>
```
