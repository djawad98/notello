<script setup lang="ts">
import { ref } from 'vue'
type Note = 'do-start' | 'do-end' | 're' | 'mi' | 'fa' | 'sol' | 'la' | 'si';
const notes = ref<Note[]>(['do-start', 're', 'mi','re','mi', 'mi','re','mi'])
</script>

<template>
  <main>
    <ul class="board">
      <img v-for="note,i of notes" :key="i" :src="`notes/${note}.gif`">
    </ul>
  </main>
</template>


<style lang="scss" scoped>
@use 'sass:math';
$height: 40px;
.board {
  @apply flex;
}
</style>