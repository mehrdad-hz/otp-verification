<template>
  <div>
    <OTPVerification />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import OTPVerification from "@/components/OTPVerification.vue";

export default defineComponent({
  name: "App",
  components: {
    OTPVerification,
  },
});
</script>

<style lang="scss"></style>
