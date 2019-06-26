<template>
    <div>
        <div class="container-fluid h-100">
            <nav class="navbar navbar-expand-lg fixed-top color">
                <!-- <img src="https://webassets.mongodb.com/_com_assets/global/mongodb-logo-white.png" height="42" style="margin-right: 10px"> -->
                <a class="navbar-brand color shadow p-3 rounded">MongoDB Surveys</a>
                <div class="navbar-brand" id="header"> </div>
            </nav>
            <form>
                <br>
                <a href="/addSurvey.html"><button type="button" class="btn btn-primary">Create New Survey</button></a>
                <br><br>
            </form>
            <div class="row h-100">
                <div class="col-12 color-sec" id="surveyList">
                    <ul id="survey-list">
                        <li><span v-html="surveyHeader"></span></li>
                        <li v-for="survey in surveys" v-bind:key="survey.surveyName">
                            <div class="row h-12">
                                <div class="col-md-2"><small> {{survey.surveyName}} </small></div>
                                <div class="col-md-3"><small> {{survey.surveyTitle}} </small></div>
                                <div class="col-md-2"><small> {{survey.lastUpdatedBy}} </small></div>
                                <div class="col-md-2"><small> {{survey.date}} {{survey.time}} </small></div>
                                <div class="col-md-1"><small><a :href="'/index.html?surveyName=' + survey.surveyName">Survey</a></small></div>
                                <div class="col-md-1"><small><a :href="'/surveyResults.html?surveyName=' + survey.surveyName">Results</a></small></div>
                                <div class="col-md-1"><small><a :href="'/addSurvey.html?surveyName=' + survey.surveyName">Edit</a></small></div>
                                <!-- <div class="col-md-1"><a href="/deleteSurvey.html?surveyName={{survey.surveyName}}">Delete</a></div> -->
                            </div>
                        </li>
                    </ul> 
                </div>
                <div id="writeStatus"></div>
            </div>
        </div>
    </div>
</template>

<script>
// import { setPriority } from 'os';
import {//Stitch,
        RemoteMongoClient
} from "mongodb-stitch-browser-sdk"

export default {
    name: "SurveyList",
    props: ["stitchClient"],
    data() {
        return {
            error: '',
            progress: '',
            surveyHeader: 
                `<div class="row h-12">
                    <div class="col-md-2"><small><strong>Survey Name</strong></small></div>
                    <div class="col-md-3"><small><strong>Survey Title</strong></small></div>
                    <div class="col-md-2"><small><strong>Last Updated By</strong></small></div>
                    <div class="col-md-2"><small><strong>Last Updated</strong></small></div>
                    <div class="col-md-1"><small><strong>Survey</strong></small></div>
                    <div class="col-md-1"><small><strong>Results</strong></small></div>
                    <div class="col-md-1"><small><strong>Edit</strong><small></div>
                </div>`,
            surveys: [],
            db: '',
            dummy: ''
        }
    },
    methods: {
        setProgress(msg) {
            this.progress = msg;
            // eslint-disable-next-line
            console.log(msg);
            this.$emit('progress', this.progress);
        },
        setError(msg) {
            this.error = msg;
            // eslint-disable-next-line
            console.error(msg);
            this.$emit('error', this.error);
        },
        fetchSurveys() {
            this.setProgress("Fetching list of surveys from Atlas");
            this.db.collection("questions")
            .find({},
                {
                    projection:{_id: 0, surveyName: 1, notes: 1, lastUpdated: 1, lastUpdatedBy: 1, surveyTitle: 1},
                sort: {_id: -1}
                })
            .asArray()
            .then(docs => {
                this.surveys = docs;
                this.setProgress("Fetched the set of surveys from MongoDB Atlas.");
            })
            .catch (err => {
                this.setError(`Couldn't find the 'questions' collection in MongoDB Atlas: ${err}`);
                return;
            })
        }
    },
    created () {
        this.db = this.stitchClient
            .getServiceClient(RemoteMongoClient.factory, "mongodb-atlas")
            .db("survey");

        this.fetchSurveys();
    }
}
</script>
<style scoped>
    li {
        text-align: left;
        list-style-type: none; 
    }
</style>
