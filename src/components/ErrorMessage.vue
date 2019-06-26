<template>
    <div v-if="errorState" id="error-message" class="alert alert-danger" role="alert">
        <strong>Error: </strong>{{errorState}}
    </div>
</template>

<script>
export default {
    name: "ErrorMessage",
    props: ["errorState"]
}
</script>
