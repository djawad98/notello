<script setup lang="ts">
import { ref, watch, watchEffect } from 'vue'
import { sample } from 'underscore';

type Note = 'do-start' | 'do-end' | 're' | 'mi' | 'fa' | 'sol' | 'la' | 'si';
type Sequence = {
  id: number,
  createdAt: Date,
  label: string,
  content: Note[]
}
const allNotes: Note[] = ['do-start', 're', 'mi', 'fa', 'sol', 'la', 'si', 'do-end'];

const PREFERED_NOTES_KEY = 'notella-prefered-notes';
const preferedNotesInLocalStorage = JSON.parse(localStorage.getItem(PREFERED_NOTES_KEY) ?? '[]');
const preferedNotes = ref<Note[]>(preferedNotesInLocalStorage.length ? preferedNotesInLocalStorage : allNotes);

const PREFERED_NOTE_COUNT_KEY = 'notella-prefered-note-count';
const preferedCountInLocalStorage = JSON.parse(localStorage.getItem(PREFERED_NOTE_COUNT_KEY) ?? '0');
const noteCount = ref<number>(preferedCountInLocalStorage ?? 10);

const notes = ref<Note[]>([]);

function generate() {
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

watch(preferedNotes, (notes) => {
  localStorage.setItem(PREFERED_NOTES_KEY, JSON.stringify(notes))
})
watch(noteCount, (count) => {
  localStorage.setItem(PREFERED_NOTE_COUNT_KEY, JSON.stringify(count))
})

const SAVED_NOTE_SEQUENCE_KEY = 'notella-saved-sequences';
const savedSequencesInLocalStorage = JSON.parse(localStorage.getItem(SAVED_NOTE_SEQUENCE_KEY) ?? '[]')
const savedSequences = ref<Sequence[]>(savedSequencesInLocalStorage)
function saveSequence() {
  const sequenceName = prompt('Enter the name:');
  if (sequenceName) {
    savedSequences.value.push({
      label: sequenceName,
      createdAt: new Date(),
      content: notes.value,
      id: new Date().getTime()
    })

    localStorage.setItem(SAVED_NOTE_SEQUENCE_KEY, JSON.stringify(savedSequences.value))
  }
}

function loadSequence(sequence: Sequence) {
  notes.value = sequence.content
}

function deleteSequence(deletedSequence: Sequence) {
  if (confirm('Are you sure?')) {
    savedSequences.value = savedSequences.value.filter(seq => {
      return seq.id !== deletedSequence.id
    })

    localStorage.setItem(SAVED_NOTE_SEQUENCE_KEY, JSON.stringify(savedSequences.value))
  }
}
</script>

<template>
  <main>
    <div class="p-2 border-b border-gray-300">
      <h1 class="text-center text-xl font-bold">Notello</h1>
      <p class="text-center text-sm text-gray-400">Learn music your way</p>
    </div>
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
      <button @click="generate" type="button" class="button">Generate</button>
    </div>
    <div class="max-w-full overflow-y-auto pb-4">
      <ul class="board">
        <img v-for="note, i of notes.slice(0, noteCount)" :key="i" :src="`notes/${note}.gif`">
      </ul>
    </div>
    <div class="p-4">
      <button @click="saveSequence" type="button" class="button w-full mb-4">Save</button>
      <div class="mt-4">
        <p>Saved Sequences:</p>
        <ul v-if="savedSequences.length" class="list">
          <li v-for="sequence, i in savedSequences" :key="i" @click="loadSequence(sequence)">
            {{ sequence.label }}
            <button @click.stop="deleteSequence(sequence)" type="button"
              class="text-sm text-red-500 hover:underline">Delete</button>
          </li>
        </ul>
        <p v-else class="text-sm mt-2 text-gray-400">No sequence found.</p>
      </div>
    </div>
  </main>
</template>


<style lang="scss" scoped>
.board {
  @apply flex;
}

.button {
  @apply px-4 py-1 rounded border border-solid border-gray-200;
}

.list {
  @apply flex flex-col divide-gray-400 divide-y;

  li {
    @apply px-2 py-1 cursor-pointer;
    @apply flex justify-between items-center
  }
}
</style>