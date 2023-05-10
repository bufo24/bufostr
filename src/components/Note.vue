<script setup lang="ts">
defineProps<{ pubkey: string; text: string }>();
</script>

<template>
  <div class="note">
    <h3>{{ pubkey }}</h3>

    <p>{{ text }}</p>
  </div>
</template>

<style scoped>
.note {
  border: 2px solid white;
  margin: 5px;
}
</style>
