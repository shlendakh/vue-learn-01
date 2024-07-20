<script setup>
import JobListings from "../components/JobListings.vue";
import { useRoute } from "vue-router";

const route = useRoute();
const message = route.query.message;

</script>

<template>
    <JobListings />
</template>