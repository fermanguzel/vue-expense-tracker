<script setup lang="ts">
import { defineProps } from "vue";

const props = defineProps({
  total: {
    type: Number,
    required: true,
  },
});
</script>

<template>
  <h4>Your Balance</h4>
  <h1 id="balance">€{{ total }}</h1>
</template>
