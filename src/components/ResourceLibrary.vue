<template>
  <div>
    <h1>Resource Hub 2.0</h1>
    <div class="items-container">
      <div v-for="item in items" :key="item.id" class="item">
        <h2>{{ item.Title }}</h2>
        <p>{{ item.Content }}</p>
        <p>{{ item.Categories }}</p>
        <p>Tagged:{{ item.Tags }}</p>
        <p><a href="{{item.link}}">{{ item.Link }}</a></p>
      </div>
    </div>
  </div>
</template>

<style>
.items-container {
  /* Styles for the container */
}

.item {
  border: 1px solid #ccc; /* Example: Adds a border around each item */
  padding: 20px; /* Example: Adds padding around each item */
}

.item {
  /* Styles for each item, e.g., borders, padding, etc. */
  margin-bottom: 20px; /* Example: Adds space between items */
}
</style>

<script>
import axios from 'axios';

export default {
  name: 'FetchData',
  data() {
    return {
      items: []
    };
  },
  mounted() {
    axios.get('http://localhost:3001/data')
      .then(response => {
        this.items = response.data.data;
      })
      .catch(error => {
        console.error("There was an error fetching the data:", error);
      });
  }
};
</script>
