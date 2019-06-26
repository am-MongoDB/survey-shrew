<template>
    <div v-if="progressState" id="progress-message" class="alert alert-info" role="alert">
        <strong>Working on it: </strong>{{progressState}}
    </div>
</template>

<script>
export default {
    name: "ProgressMessage",
    props: ["progressState"]
}
</script>
