<script setup lang="ts">
import { ref } from 'vue'

const world = ref('world')
</script>

<template>
  <h1 class="text-2xl">Hello {{ world }}</h1>
</template>
