<template>
  <div v-if="event">
    <h1>{{ event.title }}</h1>
    <p>{{ event.time }} on {{ event.date }} @ {{ event.location }}</p>
    <p>{{ event.description }}</p>
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
import EventService from "../services/EventService";
export default {
  props: ["id"],
  data() {
    return {
      event: null,
    };
  },
  created() {
    setTimeout(() => {
      EventService.getEvent(this.id)
        .then((response) => {
          console.log("Fetching event id:", response.data.id);
          this.event = response.data;
        })
        .catch((error) => {
          console.log("There was an error:", error);
        });
    }, 200);
  },
};
</script>

<style scoped>
</style>