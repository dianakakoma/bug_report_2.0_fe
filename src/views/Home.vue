<template>
  <div class="home">
        <p>
      <img src="@/assets/happy-ladybug.png" width="100" />
    </p>
    <h1>{{ message }}</h1>
    <h2>A bug reporting tool built for the end user!</h2>
    <!-- create action -->
    <h1>File a New Report</h1>
    <div align="left">
      Name: <input type="text" v-model="newName" /> <br>
      Description: <input type="text" v-model="newDescription" /><br>
      URL: <input type="text" v-model="newURL" /><br>
      Suggested Fix: <input type="text" v-model="newSuggestedFix"/><br>
      Screenshot: <input type="text" v-model="newScreenshot"/> <br>
      <button style="background-color:green;color:white" v-on:click="createReport()">Create New Report</button>
    </div>
    <!--Index Action -->
    <h2>Existing Reports</h2>
    <div v-for="report in reports" align="left">
      <p>Report Id: {{report.id}}</p>
       <p>Reported by: {{report.name}}</p>
       <p>Description:  {{report.description}}</p>
    <!-- Show Action -->
    
      <p>URL:   <a v-bind:href="report.url">{{report.url}}</a></p>
      <img v-bind:src="report.screenshot" v-bind:alt="Screenshot" width="200px"/>
      <br>
      <button style="background-color:blue;color:white" v-on:click="showReport(report)">Show more</button>
      <div v-if="currentReport === report">
        <p>Suggested fix:   {{report.suggested_fix}}</p>
        <p>Status:  {{report.status}}</p>

    <!-- Update Action -->
        <div>
          Update the Suggested Fix:<input type="text" v-model="report.suggested_fix"/>
          <button style="background-color:black;color:white" v-on:click="updateReport(report)">Update Report</button>
        </div>
        <button style="background-color:pink;color:black"v-on:click="destroyReport(report)">Destroy This Report</button>
      </div>
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
      currentReport: {},
      newName: "",
      newDescription: "",
      newURL: "",
      newSuggestedFix: "",
      newScreenshot: "",
    };
  },
  created: function () {
    axios.get("/api/reports").then((response) => {
      this.reports = response.data;
    });
  },
  methods: {
    createReport: function () {
      var params = {
        name: this.newName,
        description: this.newDescription,
        URL: this.newURL,
        suggested_fix: this.newSuggestedFix,
        screenshot: this.newScreenshot,
      };
      axios.post("/api/reports", params).then((response) => {
        this.reports.shift(response.data);
        this.newName = "";
        this.newURL = "";
        this.newDescription = "";
        this.newSuggestedFix = "";
        this.newScreenshot = "";
      });
    },
    showReport: function (report) {
      if (this.currentReport === report) {
        this.currentReport = {};
      } else {
        this.currentReport = report;
      }
    },
    updateReport: function (report) {
      var params = {
        suggested_fix: report.suggested_fix,
      };
      axios.patch("/api/reports/" + report.id, params).then((response) => {
        this.currentReport = {};
      });
    },
    destroyReport: function (report) {
      axios.delete("/api/reports/" + report.id).then((response) => {
        var index = this.reports.indexOf(report);
        this.reports.splice(index, 1);
      });
    },
  },
};
</script>
