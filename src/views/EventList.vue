<template>
  <div v-if="events" class="events">
    <h1>Event for Good</h1>
    <EventCard v-for="event in events" :key="event.id" :event="event" />
  </div>
  <div v-else class="lds-roller">
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
  </div>
</template>

<script>
import EventCard from "@/components/EventCard.vue";
import EventService from "../services/EventService";
export default {
  name: "EventList",
  components: {
    EventCard,
  },
  data() {
    return {
      events: null,
    };
  },
  created() {
    setTimeout(() => {
      EventService.getEvents()
        .then((response) => {
          console.log(`Fetching ${response.data.length} events`);
          this.events = response.data;
        })
        .catch((error) => {
          console.log("There was an error: " + error.message);
        });
    }, 1000);
  },
};
</script>

<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
