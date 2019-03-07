<template>
  <div class="container">
    <div class="search-form">
      <md-field>
        <label>Search a Website</label>
        <md-input v-model="search"/>
      </md-field>
      <md-button v-on:click="loadPreview" class="md-raised md-primary">Search</md-button>
    </div>
    <div v-show="loading">loading...</div>
    <md-card class="result" v-if="result !== null">
      <md-card-header>
        <md-card-header-text>
          <div class="md-title">{{result.title}}</div>
          <div class="md-subhead">{{result.description || 'No description'}}</div>
        </md-card-header-text>
      </md-card-header>
      <md-card-media>
        <img v-bind:src="result.image" alt="No image found">
      </md-card-media>
      <md-card-actions>
        <md-button v-bind:href="fullUrl">Go There</md-button>
      </md-card-actions>
    </md-card>
  </div>
</template>

<script>
import axios from "axios";

// Be nice, don't suck
const DEFINITELY_NOT_AN_4P1_K3Y =
  "5c7ecdb3e057229579d096935b2f86a4d4f1be47cb69c";

export default {
  name: "Search",
  data: () => ({
    search: "",
    loading: false,
    result: null,
    error: false
  }),
  computed: {
    fullUrl() {
      return `http://${this.search}`;
    }
  },
  methods: {
    loadPreview() {
      this.loading = true;
      this.error = axios
        .get(
          `http://api.linkpreview.net/?key=${DEFINITELY_NOT_AN_4P1_K3Y}&q=${
            this.search
          }`
        )
        .then(response => {
          console.log(response);
          this.result = response.data;
          this.loading = false;
          this.error = false;
          return false;
        })
        .catch(error => {
          this.result = error.response.data;
          this.loading = false;
          return true;
        });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.search-form {
  display: flex;
  max-width: 400px;
  align-items: center;
}
.result {
  max-width: 400px;
}
.container {
  margin: 20px;
}
</style>
