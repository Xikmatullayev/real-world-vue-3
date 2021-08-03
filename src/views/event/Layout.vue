<template>
  <div v-if="event">
    <h1>{{ event.title }}</h1>

    <div id="nav">
      <router-link :to="{ name: 'EventDetails' }"> Details </router-link>
      |
      <router-link :to="{ name: 'EventRegister' }"> Register </router-link>
      |
      <router-link :to="{ name: 'EventEdit' }"> Edit </router-link>
    </div>

    <router-view :event="event" />
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
import EventService from "../../services/EventService";
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
          if (error.response && error.response.status == 404) {
            this.$router.push({
              name: "404Resource",
              params: { resource: "event" },
            });
          } else {
            this.$router.push({
              name: "NetworkError",
            });
          }
        });
    }, 200);
  },
};
</script>

<style scoped>
</style>