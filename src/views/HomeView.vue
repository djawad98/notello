<script setup lang="ts">
import { ref, watch, watchEffect } from 'vue'
import { sample } from 'underscore';

type Note = 'do-start' | 'do-end' | 're' | 'mi' | 'fa' | 'sol' | 'la' | 'si';
const allNotes: Note[] = ['do-start', 're', 'mi', 'fa', 'sol', 'la', 'si', 'do-end'];
const preferedNotes = ref<Note[]>(allNotes);
const noteCount = ref<number>(10);
const notes = ref<Note[]>([]);

function generate() {
  console.log('genereate')
  let samplee = new Array(noteCount.value).fill('');
  const notess = samplee.map(item => {
    item = sample(preferedNotes.value)
    return item;
  })
  notes.value = notess
}
watchEffect(() => {
  generate()
})
</script>

<template>
  <main>
    <div class="p-4 flex flex-col gap-4">
      <ul class="flex flex-wrap">
        <li v-for="note, i in allNotes" :key="i" class="block w-1/2">
          <label :for="note">
            <input type="checkbox" v-model="preferedNotes" :value="note" :id="note">
            <span class="ml-1">{{ note }}</span>
          </label>
        </li>
      </ul>
      <input type="number" v-model="noteCount" min="1">
      <button @click="generate" class="px-4 py-1 rounded border border-solid border-gray-200">Generate</button>
    </div>
    <div class="max-w-full overflow-y-auto">
    <ul class="board">
      <img v-for="note, i of notes.slice(0, noteCount)" :key="i" :src="`notes/${note}.gif`">
    </ul>
  </div>
  </main>
</template>


<style lang="scss" scoped>
@use 'sass:math';
$height: 40px;

.board {
  @apply flex;
}
</style>