<template>
  <div class="home">
    <AnonymousAuth v-bind:originalStitchClient="stitchClient" v-on:stitch-client="setStitchClient" v-on:db="setDb" v-on:error="setError"> </AnonymousAuth>
    <div id="survey-list-container">
      <SurveyList v-bind:db="db" v-on:error="setError" v-on:success="setOK" v-on:progress="setProgress"/>
    </div>
    <div id="status-message">
        <ProgressMessage v-bind:progressState="progressState"/>
        <ErrorMessage v-bind:errorState="errorState"/>
        <OKMessage v-bind:okState="okState" v-on:success="setOK"/>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import OKMessage from '../components/OKMessage.vue'
import ErrorMessage from '../components/ErrorMessage.vue'
import ProgressMessage from '../components/ProgressMessage.vue'
import AnonymousAuth from '../components/AnonymousAuth.vue'
import SurveyList from '../components/SurveyList.vue'

export default {
  name: 'home',
  // props: ["stitchClient", "test"],
  components: {
    ErrorMessage,
    ProgressMessage,
    OKMessage,
    AnonymousAuth,
    SurveyList
  },
  data() {
    return {
      errorState: '',
      progressState: '',
      okState: '',
      db: '',
      stitchClient: ''
      // stitchClientID: ''
    }
  },
  methods: {
    setOK(message) {
      this.okState = message;
      this.errorState = '';
      this.progressState = '';
    },
    setError(message) {
      this.okState = '';
      this.errorState = message;
      this.progressState = '';
    },
    setProgress(message) {
      this.okState = '';
      this.errorState = '';
      this.progressState = message;
    },
    setStitchClient(client) {
      this.stitchClient = client;
      this.setProgress('Set the Stitch Client');
    },
    setDb(db) {
      this.db = db;
      this.setProgress('Connected to the database');
    }
  },
  mounted() {
  }
}
</script>