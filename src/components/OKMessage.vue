<template>
    <div v-if="okState" id="ok-message" class="alert alert-success" role="alert">
        <strong>Success: </strong>{{okState}}
    </div>
</template>

<script>
export default {
    name: "OKMessage",
    props: ["okState"],
    updated() {
        setTimeout(() => {
            this.$emit('success', '');
        }, 5000)
    }
}
</script>
