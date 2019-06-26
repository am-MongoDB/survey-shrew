<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <AnonymousAuth v-bind:originalStitchClient="stitchClient" v-on:stitch-client="setStitchClient" v-on:db="setDb" v-on:error="setError"> </AnonymousAuth>
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    <!-- <div v-if="!loggedIn()" id="stitch-login">
      <form @submit="loginGoogle">
          <input type="submit" value="Log in using Goolge" class="btn">
      </form>
    </div> -->
    <div id="status-message">
        <ProgressMessage v-bind:progressState="progressState"/>
        <ErrorMessage v-bind:errorState="errorState"/>
        <OKMessage v-bind:okState="okState" v-on:success="setOK"/>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
// import stitch from 'mongodb-stitch-browser-sdk/dist/browser/stitch'
// import stitch from 'https://s3.amazonaws.com/stitch-sdks/js/bundles/4.0.8/stitch.js'

import HelloWorld from '@/components/HelloWorld.vue'
import OKMessage from '../components/OKMessage.vue'
import ErrorMessage from '../components/ErrorMessage.vue'
import ProgressMessage from '../components/ProgressMessage.vue'
import AnonymousAuth from '../components/AnonymousAuth.vue'
// import {Stitch,
        // AnonymousCredential
        // GoogleCredential,
        // GoogleRedirectCredential
        // RemoteMongoClient
        // } from "mongodb-stitch-browser-sdk"
//import stitch from 'mongodb-stitch-server-sdk'
//import stitch from '../../node_modules/mongodb-stitch-browser-sdk'
//import stitch from 'mongodb-stitch/dist/stitch.js'
// src="https://s3.amazonaws.com/stitch-sdks/js/bundles/4.0.8/stitch.js";

export default {
  name: 'home',
  // props: ["stitchClient", "test"],
  components: {
    HelloWorld,
    ErrorMessage,
    ProgressMessage,
    OKMessage,
    AnonymousAuth
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
      this.setOK('Set the Stitch Client');
    },
    setDb(db) {
      this.db = db;
      this.setOK('Connected to the database');
    }

    // loggedIn() {
    //   return this.stitchClientID !== '';
    // }, 
    // loginGoogle() {
    //     if (!this.stitchClient) {
    //       try {
    //         this.setProgress('Attempting to register with Stitch app');
    //         if (Stitch.defaultAppClient) {
    //           this.stitchClient = Stitch.defaultAppClient
    //         } else {
    //           this.stitchClient = Stitch.initializeDefaultAppClient("survey-xyuov");
    //         }
    //         if (this.stitchClient) {
    //           this.setProgress("Registered with Stitch app")
    //         } else {
    //           this.setError("Failed to register with Stitch app");
    //           return;
    //         }
    //       }
    //       catch (err) {
    //         this.setError(`Failed to register with Stitch app: ${err.message}`);
    //         return;
    //       }
    //     }
    //     try {
    //       const credential = new GoogleRedirectCredential();
    //       this.stitchClient.auth.loginWithRedirect(credential)
    //     }
    //     catch (err) {
    //       this.setError(`Failed to log in: ${err.message}`);
    //     }
    //     this.Progress('Waiting for redirect from Google');
    // },
    // postLogin() {
    //   if (this.stitchClient) {
    //     if (this.stitchClient.auth.hasRedirectResult()) {
    //         this.stitchClient.auth.handleRedirectResult()
    //         .then(() => {
    //           try {
    //             this.stitchClientID = this.stitchClient.auth.user.id;
    //             this.db = this.stitchClient.getServiceClient(Stitch.RemoteMongoClient.factory, 
    //               "mongodb-atlas").db("survey");
    //               this.setOK('Connected to the database')
    //           }
    //           catch (err) {
    //             this.setError(`Failed to connect to database: ${err.message}`);
    //             return;
    //           }
    //         });
    //     }
    //     // Check for the case that a user is logged in anonymously; if so get
    //     // them to log in using Google.
    //     if (this.stitchClient.auth.isLoggedIn) {
    //       if(this.stitchClient.auth.currentUser.loggedInProviderType === "anon-user") {
    //         this.setProgress('Attempting to relogin with Google');
    //         try {
    //           const credential = new GoogleRedirectCredential();
    //           this.stitchClient.auth.loginWithRedirect(credential);
    //         }
    //         catch (err) {
    //           this.setError(`Failed to login with Google: ${err.message}`);
    //         }
    //       }
    //     }
    //   }
    // },
    // anonymousLogin() {
    //   // Register with Stitch
    //   if (!this.stitchClient) {
    //     try {
    //       this.setProgress('Attempting to register with Stitch app');
    //       try {
    //         if (Stitch.defaultAppClient) {
    //           this.setProgress('Default app already registered');          
    //           this.stitchClient = Stitch.defaultAppClient;
    //         } else {
    //           this.setProgress('Trying to register with app');
    //           this.stitchClient = Stitch.initializeDefaultAppClient("survey-xyuov");
    //           this.setProgress('Registered with app');
    //         }
    //       }
    //       catch (err) {
    //         // Not an actual error if trying to read the current default app failedd
    //         this.setProgress('Trying to register with app after event');
    //         this.stitchClient = Stitch.initializeDefaultAppClient("survey-xyuov");
    //         this.setProgress('Registered with app after event');
    //       }
    //       if (this.stitchClient) {
    //         this.setProgress("Registered with Stitch app")
    //       } else {
    //         this.setError("Failed to register with Stitch app");
    //         return;
    //       }
    //     }
    //     catch (err) {
    //       this.setError(`Failed to register with Stitch app: ${err.message}`);
    //       return;
    //     }
    //   }
    //   // Anonymous login
    //   if (!this.stitchClient.auth.isLoggedIn) {
    //     this.stitchClient.auth.loginWithCredential(new AnonymousCredential())
    //     .then(() => {
    //       this.setOK('Logged in anonymously with Stitch app');
    //     })
    //     .catch(err => {
    //       this.setError(`Anonymous Stitch authentication failed: ${err.message}`);
    //     });
    //   } else {
    //     this.setOK("Already logged in");
    //   }
    // }
  },
  mounted() {
    // this.postLogin();
    // this.anonymousLogin();
  //   try {
  //     this.db = this.stitchClient.getServiceClient(Stitch.RemoteMongoClient.factory, 
  //       "mongodb-atlas").db("survey");
  //       this.setOK('Connected to the database')
  //   }
  //   catch (err) {
  //     this.setError(`Failed to connect to the database: ${err.message}`)
  //   }
  }
}
</script>