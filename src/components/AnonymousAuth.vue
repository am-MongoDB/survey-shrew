<template>
    <div>

    </div>
</template>

<script>
import {Stitch,
        AnonymousCredential,
        RemoteMongoClient
    } from "mongodb-stitch-browser-sdk"

export default {
    name: "AnonymousAuth",
    props: ["stitchClient"],
    data() {
        return {
            // newStitchClient: '',
            error: '',
            db:''
        }
    },
    methods: {
        anonymousLogin() {
        // // Register with Stitch
        // if (this.originalStitchClient) {
        //     this.newStitchClient = this.originalStitchClient;
        // } else {
        //     try {
        //         try {
        //             if (Stitch.defaultAppClient) {
        //                 this.newStitchClient = Stitch.defaultAppClient;
        //             } else {
        //                 this.newStitchClient = Stitch.initializeDefaultAppClient("survey-xyuov");
        //             }
        //         }
        //         catch (err) {
        //             // Not an actual error if trying to read the current default app failedd
        //             this.newStitchClient = Stitch.initializeDefaultAppClient("survey-xyuov");
        //         }
        //         if (!this.newStitchClient) {
        //             this.error = ("Failed to register with Stitch app");
        //             return;
        //         }
        //     }
        //     catch (err) {
        //         this.error = (`Failed to register with Stitch app: ${err.message}`);
        //         return;
        //     }
        // }
        // Anonymous login
        if (!this.stitchClient.auth.isLoggedIn) {
            this.stitchClient.auth.loginWithCredential(new AnonymousCredential())
            .then(() => {
            })
            .catch(err => {
                this.error = `Anonymous Stitch authentication failed: ${err.message}`;
            });
        }
    }
    // connectDatabase() {
    //     try {
    //         // this.db = this.newStitchClient.getServiceClient(RemoteMongoClient.factory,       "mongodb-atlas").db("survey");
    //     }
    //     catch (err) {
    //         this.error = `Failed to connect to the database: ${err.message}`;
    //     }
    // }
  },
  created() {
    this.anonymousLogin();
    // if (!this.error) {
    //     this.connectDatabase();
    // }
    if (this.error) {
        this.$emit('error', this.error);
    } else {
        // this.$emit('stitch-client', this.newStitchClient);
        // this.$emit('db', this.db);
    }
  }
}
</script>
