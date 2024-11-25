<script setup>
import { onMounted } from "vue";
import { useRouter } from "vue-router";
import HomeGlassComponent from "@/components/HomeGlassComponent.vue";

const isAuthenticated = localStorage.getItem("t%20o%20k%20e%20n");
const router = useRouter();

// Automatic redirection to the dashboard if the user's session is still active
onMounted(() => {
  if (isAuthenticated) {
    router.push("/dashboard");
  }
});
</script>

<template>
  <HomeGlassComponent />
</template>

<style scoped></style>
