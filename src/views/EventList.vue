<template>
  <div v-if="events" class="events">
    <h1>Event for Good</h1>
    <EventCard v-for="event in events" :key="event.id" :event="event" />

    <div class="pagination">
      <router-link
        id="page-prev"
        :to="{ name: 'EventList', query: { page: page - 1 } }"
        rel="prev"
        v-if="page != 1"
      >
        &#60; Prev Page
      </router-link>
      <router-link
        id="page-next"
        :to="{ name: 'EventList', query: { page: page + 1 } }"
        rel="next"
        v-if="hasNextPage"
      >
        Next Page &#62;
      </router-link>
    </div>
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
  props: ["page"],
  components: {
    EventCard,
  },
  data() {
    return {
      events: null,
      totalEvents: 0,
    };
  },
  computed: {
    hasNextPage() {
      let totalPage = Math.ceil(this.totalEvents / 2);
      return totalPage > this.page;
    },
  },
  created() {
    this.events = null;
    setTimeout(() => {
      EventService.getEvents(2, this.page)
        .then((response) => {
          console.log(`Fetching ${response.data.length} events`);
          this.events = response.data;
          this.totalEvents = response.headers["x-total-count"];
        })
        .catch((error) => {
          console.log("There was an error: " + error.message);
        });
    }, 200);
  },
};
</script>

<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.pagination {
  display: flex;
  width: 290px;
  margin-top: 20px;
}
.pagination a {
  flex: 1;
  text-decoration: none;
  color: #2c3e50;
}

.pagination a:hover {
  color: #42b983;
}

#page-prev {
  text-align: left;
}

#page-next {
  text-align: right;
}
</style>
