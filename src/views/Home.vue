<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="report in reports">
      <p>Report Id: {{report.id}}</p>
       <p>Reported by: {{report.name}}</p>
       <p>Description: {{report.description}}</p>
       <br>
      URL: <a v-bind:href="report.url">{{report.url}}</a>
      <br>
      <img v-bind:src="report.screenshot" v-bind:alt="Screenshot" width="200px"/>
      <p>Suggested fix: {{report.suggested_fix}}</p>
      <h2 style="color:red">****</h2>
    </div>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Bug Report 2.0!",
      reports: [],
    };
  },
  created: function () {
    axios.get("/api/reports").then((response) => {
      this.reports = response.data;
    });
  },
  methods: {},
};
</script>
