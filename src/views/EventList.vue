<template>
  <div class="events">
    <h1>Event for Good</h1>
    <EventCard v-for="event in events" :key="event.id" :event="event" />
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
    EventService.getEvents().then((response) => {
        console.log(response.data);
        this.events = response.data;
      })
      .catch((error) => {
        console.log("There was an error: " + error.message);
      });
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
