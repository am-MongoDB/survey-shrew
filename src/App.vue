<template>
  <div id="app">
    <div id="nav">
      <!-- <router-link v-bind:to="{name: 'home', params: {stitchClient: 2, test:1}}">Home</router-link> | -->
      <!-- <router-link v-bind:to="{name: 'home'}">Home</router-link> -->
      <!-- <router-link to="/about">About</router-link> -->
    </div>
    <router-view v-bind:stitchClient="stitchClient"></router-view>
  </div>
</template>

<script>
import Vue from 'vue'
import BootstrapVue from 'bootstrap-vue'
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'
import 'survey-vue/survey-vue'
import 'survey-vue/survey.css'
import { Stitch } from "mongodb-stitch-browser-sdk";
//import stitch from 'mongodb-stitch-server-sdk'
Vue.use(BootstrapVue);

export default {
  name: "survey-shrew",
  data() {
    return {
      stitchClient: ""
    }
  },
  created() {
    this.stitchClient = Stitch.initializeDefaultAppClient("survey-xyuov");
  }
}
</script>


<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
